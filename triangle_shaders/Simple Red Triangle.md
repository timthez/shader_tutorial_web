Simple Red Triangle:

Vert:
```
#version 410

in vec3 VertexPosition;
out vec4 gl_Position;

void main() {
    gl_Position =  vec4(VertexPosition,1.0);
}
```

Frag:
``` 
#version 410

out vec4 FragColor; //Implicit

void main() {
    FragColor = vec4(1.0,0.0,0.0,1.0); 
}
```



vec4(gl_FragCoord.x/500,gl_FragCoord.y/500,0.5 ,1.0);// + 0.5*sin(timer),1.0);