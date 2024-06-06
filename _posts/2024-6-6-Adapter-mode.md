---
layout: post
title: "适配器模式"
date: 2024-6-6
tags: [design-pattern]
comments: true
author: zxy
---

## 概述

适配器模式是一种软件设计模式，旨在解决不同接口之间的兼容性问题。

**目的**：将一个类的接口转换为另一个接口，使得原本不兼容的类可以协同工作。

**主要解决的问题**：在软件系统中，需要将现有的对象放入新环境，而新环境要求的接口与现有对象不匹配。

> 使用场景

- 需要使用现有类，但其接口不符合系统需求。
- 希望创建一个可复用的类，与多个不相关的类（包括未来可能引入的类）一起工作，这些类可能没有统一的接口。
- 通过接口转换，将一个类集成到另一个类系中。

> 实现方式

- **继承或依赖**：推荐使用依赖关系，而不是继承，以保持灵活性。

> 关键代码

适配器通过继承或依赖现有对象，并实现所需的目标接口。

> 应用实例

- **电压适配器**：将 110V 电压转换为 220V，以适配不同国家的电器标准。
- **接口转换**：例如，将 Java JDK 1.1 的 Enumeration 接口转换为 1.2 的 Iterator 接口。
- **跨平台运行**：在 Linux 上运行 Windows 程序。
- **数据库连接**：Java 中的 JDBC 通过适配器模式与不同类型的数据库进行交互。

> 优点

- 促进了类之间的协同工作，即使它们没有直接的关联。
- 提高了类的复用性。
- 增加了类的透明度。
- 提供了良好的灵活性。

> 缺点

- 过度使用适配器可能导致系统结构混乱，难以理解和维护。
- 在 Java 中，由于只能继承一个类，因此只能适配一个类，且目标类必须是抽象的。

> 使用建议

- 适配器模式应谨慎使用，特别是在详细设计阶段，它更多地用于解决现有系统的问题。
- 在考虑修改一个正常运行的系统接口时，适配器模式是一个合适的选择。

通过这种方式，适配器模式可以清晰地表达其核心概念和应用，同时避免了不必要的复杂性。

> 结构

适配器模式包含以下几个主要角色：

- **目标接口（Target）**：定义客户需要的接口。
- **适配者类（Adaptee）**：定义一个已经存在的接口，这个接口需要适配。
- **适配器类（Adapter）**：实现目标接口，并通过组合或继承的方式调用适配者类中的方法，从而实现目标接口。

## 实现

我们有一个 _MediaPlayer_ 接口和一个实现了 _MediaPlayer_ 接口的实体类 _AudioPlayer_。默认情况下，_AudioPlayer_ 可以播放 mp3 格式的音频文件。

我们还有另一个接口 _AdvancedMediaPlayer_ 和实现了 _AdvancedMediaPlayer_ 接口的实体类。该类可以播放 vlc 和 mp4 格式的文件。

我们想要让 _AudioPlayer_ 播放其他格式的音频文件。为了实现这个功能，我们需要创建一个实现了 _MediaPlayer_ 接口的适配器类 _MediaAdapter_，并使用 _AdvancedMediaPlayer_ 对象来播放所需的格式。

_AudioPlayer_ 使用适配器类 _MediaAdapter_ 传递所需的音频类型，不需要知道能播放所需格式音频的实际类。_AdapterPatternDemo_ 类使用 _AudioPlayer_ 类来播放各种格式。

![适配器模式的 UML 图](https://www.runoob.com/wp-content/uploads/2014/08/20210223-adapter.png)

### 步骤 1

为媒体播放器和更高级的媒体播放器创建接口。

```java
MediaPlayer.java
public interface MediaPlayer {
   public void play(String audioType, String fileName);
}

AdvancedMediaPlayer.java
public interface AdvancedMediaPlayer {
   public void playVlc(String fileName);
   public void playMp4(String fileName);
}
```

### 步骤 2

创建实现了 _AdvancedMediaPlayer_ 接口的实体类。

```java
VlcPlayer.java
public class VlcPlayer implements AdvancedMediaPlayer{
   @Override
   public void playVlc(String fileName) {
      System.out.println("Playing vlc file. Name: "+ fileName);
   }

   @Override
   public void playMp4(String fileName) {
      //什么也不做
   }
}

Mp4Player.java
public class Mp4Player implements AdvancedMediaPlayer{

   @Override
   public void playVlc(String fileName) {
      //什么也不做
   }

   @Override
   public void playMp4(String fileName) {
      System.out.println("Playing mp4 file. Name: "+ fileName);
   }
}
```

### 步骤 3

创建实现了 _MediaPlayer_ 接口的适配器类。

```java
MediaAdapter.java
public class MediaAdapter implements MediaPlayer {

   AdvancedMediaPlayer advancedMusicPlayer;

   public MediaAdapter(String audioType){
      if(audioType.equalsIgnoreCase("vlc") ){
         advancedMusicPlayer = new VlcPlayer();
      } else if (audioType.equalsIgnoreCase("mp4")){
         advancedMusicPlayer = new Mp4Player();
      }
   }

   @Override
   public void play(String audioType, String fileName) {
      if(audioType.equalsIgnoreCase("vlc")){
         advancedMusicPlayer.playVlc(fileName);
      }else if(audioType.equalsIgnoreCase("mp4")){
         advancedMusicPlayer.playMp4(fileName);
      }
   }
}
```

### 步骤 4

创建实现了 _MediaPlayer_ 接口的实体类。

```java
AudioPlayer.java
public class AudioPlayer implements MediaPlayer {
   MediaAdapter mediaAdapter;

   @Override
   public void play(String audioType, String fileName) {

      //播放 mp3 音乐文件的内置支持
      if(audioType.equalsIgnoreCase("mp3")){
         System.out.println("Playing mp3 file. Name: "+ fileName);
      }
      //mediaAdapter 提供了播放其他文件格式的支持
      else if(audioType.equalsIgnoreCase("vlc")
         || audioType.equalsIgnoreCase("mp4")){
         mediaAdapter = new MediaAdapter(audioType);
         mediaAdapter.play(audioType, fileName);
      }
      else{
         System.out.println("Invalid media. "+
            audioType + " format not supported");
      }
   }
}
```

### 步骤 5

使用 AudioPlayer 来播放不同类型的音频格式。

```java
AdapterPatternDemo.java
public class AdapterPatternDemo {
   public static void main(String[] args) {
      AudioPlayer audioPlayer = new AudioPlayer();

      audioPlayer.play("mp3", "beyond the horizon.mp3");
      audioPlayer.play("mp4", "alone.mp4");
      audioPlayer.play("vlc", "far far away.vlc");
      audioPlayer.play("avi", "mind me.avi");
   }
}
```
