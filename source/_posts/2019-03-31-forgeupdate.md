---


layout: post

title: "Minecraft Forge 1.13.2修改后的部分函数名"

date: 2019-03-31

tag: 日常

---



# 前言

跟大中一起修mod过程中记录一下这样子。		

缓更。



# 开始记录

```java
Minecraft.getMinecraft() 改为 Minecraft.getInstance()
```

```java
GlStateManager.color(float colorRed, float colorGreen, float colorBlue, float colorAlpha)
改为
GlStateManager.color4f(float colorRed, float colorGreen, float colorBlue, float colorAlpha)
```

本来的color方法的两个构造器（即仅传入RGB三个参数和传入RGBa四个参数）现在被分别改为了color3f和color4f。		

```java
GlStateManager.disableDepth() 改为 GlStateManager.disableDepthTest()
```

```
GlStateManager.enableDepth() 改为 GlStateManager.enableDepthTest()
```

```java
GlStateManager.enableAlphaTest() 改为 GlStateManager.enableAlphaTest()
```

```java
GlStateManager.translate(float x, float y, float z)
改为
GlStateManager.translatef(float x, float y, float z)
```

本来的translate方法的两个构造器（即传入三个float参数和传入三个double参数）现在分别改为了translatef和translated。

```
ItemStack.getTagCompound() 改为 ItemStack.getTag()
```

```
ItemStack.getRarity().rarityColor 改为 ItemStack.getRarity().color
```

```java
NBTTagCompound.getCompoundTag(String key) 改为 NBTTagCompound.getCompound(String key)
```

```java
NBTTagCompound.hasKey(String key, int type) 改为 NBTTagCompound.contains(String key, int type)
```

```
NBTTagCompound.getKeySet() 改为 NBTTagCompound.keySet()
```

```java
GuiButton
@Override
public void drawButton(Minecraft mc, int mouseX, int mouseY, float partialTicks) {
}
改为
@Override
public void render(int mouseX, int mouseY, float partialTicks) {
	Minecraft mc = Minecraft.getInstance();//mc对象在内部声明
}
```

```
NBTBase 改为 INBTBase
```

```java
net.minecraft.client.resources.IResource 移至 net.minecraft.resources.IResource
```

```java
net.minecraft.client.resources.IResourceManager 移至 net.minecraft.resources.IResourceManager
```





# 存疑改动



getItemDamage

> 原ItemStack.getItemDamage()方法删除，但原来的此方法返回的是：
>
> ```java
> public int getItemDamage(){
> 	return getItem().getDamage(this);
> }
> ```
>
> 考虑直接换为
>
> ```java
> ItemStack stackA
> stackA.getItemDamage() 改为
> stackA.getItem().getDamage(stackA)
> ```
>
> 





获取键盘输入：

> 弃用了 org.lwjgl.input.Keyboard 		
>
> 用 net.minecraft.client.util.InputMappings 进行了替换
>
> ```java
> KeyLoader.key_F4 = new KeyBinding("FastTrading ON-OFF", Keyboard.KEY_F4, "FastTrading");
> 改为
> KeyLoader.key_F4 = new KeyBinding("FastTrading ON-OFF", InputMappings.getInputByName("key.keyboard.f4").getKeyCode(), "FastTrading");
> ```
>
> 





net.minecraft.client.renderer.texture.TextureUtil.readBufferedImage:

> 似乎是删除了这个方法，但是此方法原来的返回是：
>
> ```java
> public static BufferedImage readBufferedImage(InputStream imageStream) throws IOException{
>         BufferedImage bufferedimage;
> 
>         try{
>             bufferedimage = ImageIO.read(imageStream);
>         }finally{
>             IOUtils.closeQuietly(imageStream);
>         }
> 
>         return bufferedimage;
>     }
> ```
>
> 因此考虑直接依此进行修改：
>
> ```java
> BufferedImage bufferedimage = readBufferedImage(iresource.getInputStream());
> 改为：
> BufferedImage bufferedimage = ImageIO.read(iresource.getInputStream());
> ```





播放声音：

> playSound(ISound sound) 方法改为了 play(ISound sound)：
>
> 但是在改变名称之后传参类型报错了，所以做了个强制转型如下：
>
> ```java
> mc.getSoundHandler().play((ISound) FakeSubtitleSound.getRecord(SoundEvents.ENTITY_ITEM_PICKUP, 0.5f, 0.05F, "fasttrading.subtitles.buttonswitching"));
> ```