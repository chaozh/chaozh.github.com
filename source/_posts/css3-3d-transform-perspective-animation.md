title: CSS3中的transform3D变换总结
tags:
  - CSS
  - CSS3
id: 398
categories:
  - CSS技巧
date: 2013-01-12 11:06:32
---

[技术宅的CSS3 3D transform总结](http://www.zhangxinxu.com/wordpress/2012/09/css3-3d-transform-perspective-animate-transition/ "好吧，CSS3 3D transform变换，不过如此！")

<pre rel="CSS3">
#secondary li{
	-webkit-perspective: 400px;
	-moz-perspective: 400px;
	-o-perspective: 400px;
	perspective: 400px;
}
#secondary a {
	display: inline-block;
	position: relative;
	-webkit-transform-style: preserve-3d;
	-moz-transform-style: preserve-3d;
	-o-transform-style: preserve-3d;
	transform-style: preserve-3d;
	-webkit-transform-origin: 50% 0;
	-moz-transform-origin: 50% 0;
	-o-transform-origin: 50% 0;
	transform-origin: 50% 0;
}
#secondary li:hover a {
	-webkit-transform: translate3d(0px,0px,-30px) rotateX(90deg);
	-moz-transform: translate3d(0px,0px,-30px) rotateX(90deg);
	-o-transform: translate3d(0px,0px,-30px) rotateX(90deg);
	transform: translate3d(0px,0px,-30px) rotateX(90deg);
}
#secondary a::after {
	content: attr(title);
	display: block;
	position: absolute;
	left: 0;
	top: 0;
	color: white;
	background: #333;
	-webkit-transform-origin: 50% 0;
	-moz-transform-origin: 50% 0;
	-o-transform-origin: 50% 0;
	transform-origin: 50% 0;
	-webkit-transform: translate3d(0px,105%,0px) rotateX(-90deg);
	-moz-transform: translate3d(0px,105%,0px) rotateX(-90deg);
	-o-transform: translate3d(0px,105%,0px) rotateX(-90deg);
	transform: translate3d(0px,105%,0px) rotateX(-90deg);
	opacity:1;
}
</pre>