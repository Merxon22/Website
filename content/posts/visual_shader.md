---
title: '不用编程的Visual Shader'
summary: '使用Godot的可视化着色器就不用编程啦'
tags: ['#着色器', '#3D', '#Visual Shader']
categories: ["着色器教程"]
cover:
    image: "visual_shader/cover.gif"
---

---
### 你将学习📖
>- Godot可视化着色器的基础
>- 使用噪声纹理制作3D模型的溶解效果
>- 使用Expression节点在可视化着色器里嵌入代码

---
### 视频教程🖥️
{{< bilibili "BV1jD421u7X3" >}}

---
### 资源下载📥

- #### 噪声纹理（右键保存）：
![]({{< resource "/visual_shader/noise.png" >}})

- 噪声代码
``` GLSL {linenos=true}
vec2 modulo(vec2 divident, vec2 divisor){
	vec2 positiveDivident = mod(divident, divisor) + divisor;
	return mod(positiveDivident, divisor);
}

vec2 random(vec2 value){
	value = vec2( dot(value, vec2(127.2,311.7) ),
				  dot(value, vec2(269.5,183.3) ) );
	return -1.0 + 2.0 * fract(sin(value) * 43758.5453123);
}

float seamless_noise(vec2 uv, float s) {
	uv = uv * s;
	vec2 cellsMinimum = floor(uv);
	vec2 cellsMaximum = ceil(uv);
	vec2 uv_fract = fract(uv);
	
	cellsMinimum = modulo(cellsMinimum, vec2(s));
	cellsMaximum = modulo(cellsMaximum, vec2(s));
	
	vec2 blur = smoothstep(0.0, 1.0, uv_fract);
	
	vec2 lowerLeftDirection = random(vec2(cellsMinimum.x, cellsMinimum.y));
	vec2 lowerRightDirection = random(vec2(cellsMaximum.x, cellsMinimum.y));
	vec2 upperLeftDirection = random(vec2(cellsMinimum.x, cellsMaximum.y));
	vec2 upperRightDirection = random(vec2(cellsMaximum.x, cellsMaximum.y));
	
	vec2 fraction = fract(uv);
	
	return clamp(mix( mix( dot( lowerLeftDirection, fraction - vec2(0, 0) ),
                     dot( lowerRightDirection, fraction - vec2(1, 0) ), blur.x),
                mix( dot( upperLeftDirection, fraction - vec2(0, 1) ),
                     dot( upperRightDirection, fraction - vec2(1, 1) ), blur.x), blur.y) * 0.8 + 0.5, 0, 1);
}
```

- #### 成品链接（下载后解压）
![]({{< resource "/visual_shader/final.gif" >}})
{{< button "下载成品项目" "/visual_shader/final.zip" >}}