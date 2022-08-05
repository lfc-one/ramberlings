---
title: "Java 9 - String concatenation"
date: 2022-08-05
---
One of the improvments in Java 9 was in how String concatenation is handled.  [JEP 280](https://openjdk.org/jeps/280) purposed a change to how Java produced byte code when we concatenated two or more string together uing the '+'.


    public String stringConcatWithPlus2(String v){
        String s = "This " + v + " eats " + "dogs";
        return s;
    }

In Java 8 this will produce byte code that uses **StringBuilder** to append each of the strings together.
  
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

  From Java 9 the byte code produced uses *invokedynamic*.
  
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
  
As the JEP states this was done to allow for optimizing of the *String* concatenation handlers, and by using *invokeDynamic* this means any future changes will have not impact to the byte code making changes hidden from the call site.

