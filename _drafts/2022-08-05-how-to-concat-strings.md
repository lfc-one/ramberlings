---
title: "How shouuld I concat Strings in Java?"
date: 2022-08-05
---

Java provides multiple ways to concatenate strings.  But which is the best way to do this.

## Concat using StringBuilder?

Is the best way to concatenate strings together to use **StringBuilder** or **StringBuffer**, well it could be, by using these we get a nice ability to chain *append* 
methods together.

    StringBuilder builder = new StringBuilder();
    builder.append("My name is).append(name).append("and I live in a").append(buildingType);
    
## Concat using +?

There is some belief that using + is worse than using **StringBuilder**.  But is that the case?  In Java 8 this is no worse than using a **StringBuilder**.  If you look
at the byte code generated it uses a **StringBuilder** when concatenating multiple strings.

> NEW java/lang/StringBuilder     
> DUP  
> INVOKESPECIAL java/lang/StringBuilder.<init> ()  
> LDC "This "  
> INVOKEVIRTUAL java/lang/StringBuilder.append (Ljava/lang/String;)Ljava/lang/StringBuilder;  
> ALOAD 1  
> INVOKEVIRTUAL java/lang/StringBuilder.append (Ljava/lang/String;)Ljava/lang/StringBuilder;  
> LDC " eats dogs"  
> INVOKEVIRTUAL java/lang/StringBuilder.append (Ljava/lang/String;)Ljava/lang/StringBuilder;  
> INVOKEVIRTUAL java/lang/StringBuilder.toString ()Ljava/lang/String;  
> ASTORE 2  
  
But things get better in Java 9 onwards.  To improve string concatenation the byte code produced now uses the **invokeDynamic** so there is no new **StringBuilder**.
  
> LINENUMBER 12 L0  
> ALOAD 1  
> INVOKEDYNAMIC makeConcatWithConstants(Ljava/lang/String;)Ljava/lang/String; [  
> // handle kind 0x6 : INVOKESTATIC  
>  
> java/lang/invoke/StringConcatFactory.makeConcatWithConstants(Ljava/lang/invoke/MethodHandles$Lookup;Ljava/lang/String;Ljava/lang/invoke/MethodType;Ljava/lang/String;[Ljava/lang/Object;)Ljava/lang/invoke/CallSite;  
> // arguments:  
> "This \u0001 eats dogs"  
> ]  
> ASTORE 2  
  
  So my recomendation is that you start using '+' for string concatenation so that we allow Java to produce the best code and you can write less code.  The worst case 
  if you are still using Java 8 is you create a new **StringBuilder** which you probably were going to do anyway.
  
  If you would like to see a more indepth look at the Java 9 improvments to *String concatenation* then have a look at my post **Java 9 - String Concatenation **
