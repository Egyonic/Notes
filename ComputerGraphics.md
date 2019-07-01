c

*libboost_date_time-vc141-mt-gd-x32-1_70.lib*

1Th#gVJSa8VmSvxX

**node-v10.16.0-linux-x64**

* [B站](https://www.bilibili.com)

# 三维图形学复习

### 几何流水线(二维/三维)
#### 二维
 * glMatrixMode (GL_PROJECTION); 将指定投影矩阵作为当前矩阵。

* glLoadIdentity(); 初始化，重置矩阵为单位矩阵
* 定义一个二维裁剪窗口  gluOrtho2D(xwmin, xwmax, ywmin, ywmax,)
* glViewport (xvmin, yvmin, vpWidth, vpHeight)  参数xvmin和yvmin指定视口左下角的位置， 参数 vpWidth和 vpHeight设定视口的宽度像素数和高度像素数

##### 窗口设置

```c++
 	glutInitWindowPosition( int x,int y) //指定了窗口左上角的屏幕位置。
	glutInitWindowSize( int width,int size); //指定了窗口的大小（以像素为单位）
	glutCreateWindow( char *string);//创建了一个带有OpenGL渲染环境的窗口。
	
	设定GLUT显示窗口的模式和颜色
	glutInitDisplayMode(unsigned int mode) //指定使用RGBA还是颜色索引模式, 指定使用单缓冲还是双缓冲窗口。
	//例子
	1. glutInitDisplayMode(GLUT_SINGLE| GLUT_RGB) glFlush( );
	2. glutInitDisplayMode(GLUT_DOUBLE| GLUT_RGB) glSwapBuffer( );
```



#### 三维

##### 观察流水线

1. 观察平面法向量

   > 观察平面也称为投影平面
   > 观察平面的方向及正zview轴可定义为观察平面法向量N

2. 观察向上向量

   > 该向量用来确定yview轴的正向
   >
   > 一种方便的选择是使用平行于世界坐标系轴yw的方向，即V=(0,1,0)

3. uvn观察坐标系

4. 生成三维观察效果



##### 投影变换

正交投影

> 易于排列或测量空间物体

透视投影

> 适用情况:
  	使图像看起来比较真实
  	能在场景中移动并具有与人眼类似的观察效果
  	不需要测量或排列图像中的对象



#### OpenGL三维观察函数

```c++
	gluLookAt(x0 , y0, z0, xref, yref, zref, Vx, Vy, Vz) 	//三组参数为相机位置，参考点，向上向量
	//指定了观察点的位置，定义了一个照相机所瞄准的参考点，并且提示哪个方向是朝上的。

    glMatrixMode( GL_PROJECTION )	//投影矩阵按OpenGL投影模式存储
        
    //正交投影参数用下列函数选择
	glOrtho( left, right, bottom, top, near, far )
        
    //OpenGL 透视投影函数
    //OpenGL默认使用glFrustum 函数; 但这个函数使用起来不方便 
    glMatrixMode(GL_PROJECTION );
	glLoadIdentity();
	gluPerspective( view_angle, aspect_ratio, front, back );
	//aspect_ratio: the aspect ratio of the view in x to that in y: (w/ h)

glFrustum( double left, double right,double bottom, double top,dobule znear, double zfar )
    
    //模板
    glMatrixMode( GL_PROJECTION);
	glLoadIdentity();
	gluPerspective( 90., 1., 0.001, 100.);
	glMatrixMode( GL_MODELVIEW);
	glLoadIdentity();
	gluLookAt( ex, ey, ez, 0., 0., 0., 0., 1., 0.);

	//示例
	void reshape(){ 
    	glMatrixMode (GL_PROJECTION);
         glLoadIdentity ();
		glFrustum (-1.0, 1.0, -1.0, 1.0, 1.5, 20.0);
		glMatrixMode (GL_MODELVIEW);
		glLoadIdentity ();
		gluLookAt (5.0, 5.0, 5.0, 0.0, 0.0, 0.0, 0.0, 1.0, 0.0);
	}
```






* opengl三个库，基本库函数

  

### 输出图元，各种类型（PPT里的图）






### 图元属性
1. 色彩模式（RGBA，颜色索引）。

####  颜色调和，打开的函数，元因子，目标因子等，调和函数的用法。 

定义颜色调和函数并打开颜色调和

```c++
glBlendFunc( GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);
glEnable( GL_BLEND);
```

3. 深度测试。

4. 虚线的画法。
   + 用glEnable(GL_LINE_STIPPLE);来启动虚线模式
   + 使用glLineStipple来设置虚线的样式，void glLineStipple(GLint factor, GLushort pattern)。 pattern为16位0/1序列，factor决定一个0或1作用的点个数，如0x00ff, 0xAAAA

5. 着色
   + glShadeModel(GL_SMOOTH); // 平滑方式，这也是默认方式
   + glShadeModel(GL_FLAT); // 单色方式

6. 走样的概念
   + 绘制时间和图像质量的平衡
   + 锯齿状的边界
   + 由于低频采样（不充分采样）而造成的信息失真称为走样
   + 校正不充分采样过程为反走样。


### 二维/三维 几何变换
1. 齐次坐标，概念
2. 变换的函数，和矩阵运算原理 （矩阵书写从右往左，运算从左往右，无影响）
	> * glTranslate{fd}(x,y,z);
	> * glRotate{fd}(angle,x,y,z);  原点和三个实数确定的另外一个点构成一条旋转轴。例：glRotatef(45.0,0.0,0.0,1.0)
	> * glScalef(x,y,z)

3. OpenGL矩阵栈的概念
	> * glPushMatrix() 复制一份当前矩阵，并把这份拷贝添加到堆栈的顶部,记住自己的位置
	> * glPopMatrix() 丢弃堆栈顶部的那个矩阵,回到原来的位置
	> * glLoadIdentity () 把当前矩阵设置为单位矩阵。绝大多数变换把当前矩阵与指定的矩阵相乘，然后把结果指定为当前矩阵如果你没有通过载入单位矩阵来清除当前矩阵，所进行的变换实际上是把当前的变换与上一次变换进行了组合


### 光照模型
#### 光照模型与面绘制算法概念

**光照明模型（illumination model）**，也称光照模型（lighting model ）或明暗模型（shading model）主要
用于对象表面某光照位置的颜色计算。
**表面绘制算法（surface-rendering method）** 是使用光照模型为对象的所有投影位置确定像素颜色。

> * 将表面上单个点的光强度计算模型称为**光照明模型**或光照模型。
> *  将应用于一个光照模型获得所有投影的表面位置的像素颜色的过程称为**表面绘制**。



环境光：在基本光照模型中，可通过设定场景一般亮度级来引入背景光。



反射光：光线直接来自光源，到达物体后，不与物体进行交互直接到达观看者，看到的颜色不是物体的而是光源的



2. 光源
	点光源衰减
	
	> 距离衰减  A为衰减因子，d为距离
	> A = 1 / (a + bd + cd^2)	
> 对无穷远光源, Af = 1 （不衰减）



#### 三种面绘制算法（恒定，Ground，Phong）

1. 恒定处理

   > • 为所有的投影点赋以相同的颜色。
   > • 为表面的一个位置如多边形的一个顶点或中心点计算三个RGB颜色分量强度。
   > • 也称为平面绘制。
   > • 可快速生成一般曲面的大致外观。

2. Gouraud明暗处理

   又称为强度插值表面绘制，通过在照明对象的表面上将光强度值进行线性插值来绘制多边形表面。

   > 1. 确定每个多边形顶点处的平均单位法向量；
   > 2.  对于每个顶点根据光照模型来计算其光强度；
   > 3.  在多边形投影区域对顶点强度进行线性插值。

3. Phong

   又称为法向量插值绘制，对法向量进行插值来取代对强度插值。强度值的更精确计算、更真实的表面高光显示及更少的马赫带效应。

   > 1. 确定每个多边形顶点处的平均单位法向量；
   > 2.  在多边形投影区域上对顶点法向量进行线性插值；
   > 3.  根据光照模型，使用插值的法向量，沿每条扫描线计算投影像素的光
   >    强度。



#### GL_LIGHT用法

* 开启光源i

  > glEnable(GL_LIGHTi); //i是从0到7之间的整数

* 开启光照渲染
  
  > glEnable(GL_LIGHTING);

* 设置光源属性
	glLight*(lightName, lightProperty, propertyValue);  glLightfv等形式
可选属性即值
GL_POSITION //光源的位置（默认（0,0,1））

> 位置(positional)光源，如台灯，位置（x, y, z, w）中w为1,方向（directional）光源，如太阳,w为0
>
> GLfloat light_position[] = { 2., 4., 6., 1.};
> glLightfv( GL_LIGHT0, GL_POSITION, light_position);

指定颜色时用（R,G,B,A）指定
	
GL_AMBIENT //环境光的颜色（默认黑色）
	
GL_DIFFUSE //散射光的颜色（默认白色或黑色）
	
GL_SPECULAR //镜面反射光（默认白色或黑色）
	
GL_CONSTANT_ATTENUATION //光线强度衰减（默认（1,0,0））
	
GL_SPOT_DIRECTION //聚光灯的方向（默认（0,0,-1））



**指定OpenGL光源的光线强度衰减系数**(只对位置光源有效)

> glLightf(GL_LIGHT5, GL_CONSTANT_ATTENUATION, 2.0); //常数，默认1.0
> glLightf(GL_LIGHT5, GL_LINEAR_ATTENUATION, 1.0);	// 一次参数，默认0
> glLightf(GL_LIGHT5, GL_QUADRATIC_ATTENUATION, 0.5); //二次参数，默认0



#### OpenGL方向光源（聚光灯）

* 截止角，GL_SPOT_CUTOFF,默认值180，除默认值外，它的取值范围[0.0, 90.0]。例：glLightf(GL_LIGHT0, GL_SPOT_CUTOFF, 45.0);
* 聚光灯方向， GL_SPOT_DIRECTION （轴线方向）默认方向（0.0, 0.0, -1.0）
* 聚光指数， GL_SPOT_EXPONENT 默认为0.0，轴线处光强最大。聚光指数越大，光源的聚焦效果越高



#### OpenGL全局光照参数

选择光照模型  glLightModelfv

> void glLightModelfv(paramName, paramValue) 
> paramName: 光照模型的类型	paramValue : 该类型值的数组地址

设置全局环境光颜色

> GLfloat lmodel_ambient[] = { 0.2, 0.2, 0.2, 1.0 };
> glLightModelfv(GL_LIGHT_MODEL_AMBIENT, lmodel_ambient);

选择光照模型2  glLightModeli(paramName, paramValue);

> paramName可以取值为：
> • GL_LIGHT_MODEL_LOCAL_VIEWER //观察点是否无穷远(GL_FALSE)
> • GL_LIGHT_MODEL_TWO_SIDE //单面还是双面光照(GL_FALSE )
> • GL_LIGHT_MODEL_COLOR_CONTROL //镜面反射颜色是否独立于非镜面反射颜色，默认情况下，镜面反射颜色在纹理映射之前完成，即值为GL_SINGLE_COLOR

#### OpenGL表面特性函数

指定材质属性的函数

```c++
glMaterialfv(surfFace, surfProperty, propertyValue);
/*
surfFace: 指定正面或背面的材质
surfProperty : 指定要设置哪种材质属性
propertyValue : 属性值的数组地址

属性说明
surfFace可取如下值：
• GL_FRONT
• GL_BACK
• GL_FRONT_AND_BACK

surfProperty可取如下值：
• GL_AMBIENT (0.2, 0.2, 0.2, 1.0)
• GL_DIFFUSE (0.8, 0.8, 0.8, 1.0)
• GL_AMBIENT_AND_DIFFUSE
• GL_SPECULAR (0.0, 0.0, 0.0, 1.0)
• GL_SHININESS (0.0)
• GL_EMISSION (0.0, 0.0, 0.1, 1.0)
*/
//例：镜面反射材质
GLfloat mat_specular[] = { 1.0, 1.0, 1.0, 1.0 };
GLfloat low_shininess[] = { 5.0 };
glMaterialfv(GL_FRONT, GL_SPECULAR, mat_specular);
glMaterialfv(GL_FRONT, GL_SHININESS, low_shininess);
```



#### OpenGL表面绘制函数

指定着色模式

```C++
glShadeModel(surfRenderingMethod);
// GL_FLAT 单色着色
// GL_SMOOTH 平滑着色
```

表面法向量

```c++
// 形式为 glNormal3*(NX,NY,NZ);
glBegin(GL_TRIANGLES);
	glNormal3f(0.0f, -1.0f, 0.0f);
	glVertex3f(0.0f, 0.0f, 60.0f);
	glVertex3f(-15.0f, 0.0f, 30.0f);
	glVertex3f(15.0f,0.0f,30.0f);
glEnd();
```



### 纹理

OpenGL标准要求图像的长宽都是2的幂

两种创建方式,

1. 用图像作为纹理图非常普遍

2. 直接使用RGB序列

#### 表面纹理映射两种方法

1. 将纹理映射到对象表面上，再到投影平面

2. 将像素区间映射到对象表面，再将该表面区域映射到纹理空间

   

从像素空间到纹理空间的映射是常用的纹理映射方法。

#### 平面纹理贴图

```c++
glBegin( GL_QUADS);
	glTexCoord2f( 0., 1.); glVertex3f( -2., 3.5, 0. );
	glTexCoord2f( 0., 0.); glVertex3f( -2., 0.0, 0.);
	glTexCoord2f( 1., 0.); glVertex3f( 2, 0.0, 0.);
	glTexCoord2f( 1., 1.); glVertex3f( 2., 3.5, 0.);
glEnd();

```

glTexCoord2f( 1., 1.)  中的x或y参数大于1时表示在该方向上重复



##### （1）二维纹理定义的函数

```c++
void glTexImage2D(GLenum target,GLint level,GLint components, GLsizei width, glsizei height,GLint border, GLenum format,GLenum type, const GLvoid *pixels);
```



##### （2）纹理控制

```c++
void glTexParameter{if}[v](GLenum target,GLenumpname,TYPE param);
//纹理反走样（纹理插值、纹理滤波）
glTexParameter*(GL_TEXTURE_2D,GL_TEXTURE_MAG_FILTER,GL_NEAREST);//取最近像素
glTexParameter*(GL_TEXTURE_2D,GL_TEXTURE_MIN_FILTER,GL_LINEAR); //GL_LINEAR，取几个像素加权平均
//纹理反卷（重复与约简）
glTexParameterfv(GL_TEXTURE_2D,GL_TEXTURE_WRAP_S,GL_REPEAT);
glTexParameterfv(GL_TEXTURE_2D,GL_TEXTURE_WRAP_T,GL_CLAMP);
//重复纹理（REPEAT）表示纹理坐标只关心坐标的小数部分，如果纹理坐标超过1就恢复到0；
//拉伸纹理（CLAMP）表示纹理坐标超过[0,1]的部分使用最接近0或1的值；
```

##### （3）说明映射方式

```c++
void glTexEnv{if}[v](GLenum target,GLenumpname,TYPE param);
```

纹理环境glTexEnvi（说明纹理值怎样与原来表面颜色的处理方式）

* GL_BLEND
*  GL_DECAL，用纹理色简单的替代原来的像素色
*  GL_MODULATE，用纹理色和几何像素色相乘来替代原来的像素色



##### （4）启动纹理映射

* 允许纹理映射
  glEnable(GL_TEXTURE_2D);
* 生成纹理名称
  glGenTextures(1, texName);
* 把纹理名和纹理对象绑定
  glBindTexture(GL_TEXTURE_2D,texName[0]);
* 删除纹理
  glDeleteTextures

##### （5）绘制场景，给出顶点的纹理坐标和几何坐标

```c++
// 坐标定义
void gltexCoord{1234}{sifd}[v](TYPE coords);
// 坐标自动产生
void glTexGen{if}[v](GLenum coord,GLenum pname,TYPE param);

/*纹理坐标
– 在调用glVertex()函数之前必须先说明纹理坐标
glTexCoord*()
– 纹理坐标值在[0, 1]之间*/
```

典型的2D纹理创建步骤

```c++
glEnable(GL_TEXTURE_2D);
glGenTextures(1, &texName); // define texture for sixth face
glBindTexture(GL_TEXTURE_2D,texName[0][0][0]);
glTexEnvi(GL_TEXTURE_ENV, GL_TEXTURE_ENV_MODE, GL_DECAL);
glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_WRAP_S,GL_CLAMP);
glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_WRAP_T,GL_REPEAT);
glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_MIN_FILTER,GL_LINEAR);
glTexParameteri(GL_TEXTURE_2D,GL_TEXTURE_MAG_FILTER,GL_LINEAR);
glTexImage2D(GL_TEXTURE_2D,0,GL_RGB8,TEX_WIDTH,TEX_HEIGHT,0,GL_RGB,GL_UNSIGNED_BYTE,texImage);
```






### 交互
三种交互方式（书上的实验）

1. 键盘事件交互
2. 菜单交互
3. 鼠标交互



##### GLUT鼠标函数

```c++
glutMouseFunc(mouseFcn);
Void mousFcn(GLint button,GLint action, GLint xMouse, GLintyMouse); 
//返回鼠标光标在窗口中的位置坐标（xMouse，yMouse）即相对于窗口左上角的位置。
/*
button允许值为：GLUT_LEFT_BUTTON, GLUT_MIDDLE_BUTTON,GLUT_RIGHT_BUTTON
action 允许值为：GLUT_DOWN,GLUT_UP
xMouse 为光标到窗口左边界的像素距离
yMouse 为光标到窗口上边界的像素距离
*/
```

##### GLUT键盘函数

​```c++
glutKeyboardFunc(keyFcn);
void keyFcn(Glubyte key, GLint xMouse,GLint yMouse);
//返回鼠标光标在窗口内的位置坐标(xMouse,yMouse)是相对于窗口左上角的。
/*
key 的取值是一个字符值或者对应的ASCII编码。
key 可以是：FI到F12的功能键，GLUT_KEY_F1到GLUT_KEY_F12
key 可以是：方向键，GLUT_KEY_LEFT,GLUT_KEY_UP等
key 可以是：其他特殊键，GLUT_KEY_PAGE_UP(Page up)等
*/
```

Special事件

```c++
glutSpecialFunc(functionname)
void functionname(int key,int x,int y)
/* 事件产生于“特殊键”被按下时
第一个参数是被按下的键值，第二和第三个参数是窗口整型坐标，
它记录了当特殊键被按下时光标的位置 */
```

##### OpenGL菜单功能

```c++
glutCreateMenu（meunFcn）;	//创建，设置回调函数
void meunFcn(GLint menuItemNumber),传递给menuItemNumber的整数值被menuFcn用来执行某些操作。

glutAddMenuEntry(charstring, menuItemNumber);//添加一个菜单条目，设定菜单项的名称和位置。
	//chartring设定显示在菜单上的文字
	//menuItemNumber设定该菜单项在菜单中的位置

glutAttachMenu(button); //指定用来选择菜单项的鼠标键
```

设置当前菜单：void glutSetMenu(int menu);

##### GLUT 子菜单

```c++
void glutAddSubMenu(char* name, int menu);
//在当前菜单的底部增加一个子菜单的触发条目
submenuID = glutCreateMenu(submenuFcn);
glutAddAMenuEntry(“first Submenu Item”,1);
gultCreateMenu(menuFcn);
glutAddMenuEntry(“First Menu Item ”,1);
glutAddSubMenu(“Submenu Option”,submenuID);
```

##### 修改GLUT菜单

```c++
glutDetachMenu(mouseButton);
//mouseButton是之前关联到这个菜单的鼠标键的GLUT符号常量

glutRemoveMenuItem(itemNumber);
//itemNumber被赋值为欲删除菜单项的整数标识符。
```

##### Idle事件

函数定义程序在没有其他事件处理时做什么操作，通常用来驱动实时的动画。

```c++
glutIdleFunc(functionname)
void functionname(void) //决定每一个空闲周期做什么
//这个函数的结束是调用glutPostRedisplay()
```

##### Timer事件

```c++
glutTimerFunc(msec,funtionname,value)

```

1. 需要一个整型参数(msec)来表示经过多少毫秒回调函数被触发
2. void functionname(int)
3. 还需要一个整型参数(value)作为调用该回调函数时的传入参数