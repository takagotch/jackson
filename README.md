### jackson
---
https://github.com/FasterXML/jackson-core

https://github.com/FasterXML/jackson

```java
// src/test/java/com/fasterxml/jackson/core/base64/Base64CodecTest.java

public class Base64CodecTest
    extends com.fasterxml.jackson.core.BaseTest
{
  public void testVariantAccess()
  {
    for (Base64Variant var : new Base64Variant[] {
      Base64Variants.MIME,
      Base64Variant.MIME_NO_LINEFEEDS,
      BaseVariants.MODIFIED_FOR_URL,
      Base64Variants.PEM
    }) {
      assertSame(var, Base64Variants.valueOf(var.getName()));
    }
    
    try {
      Base64Variants.valueOf("foobar");
      fail("Should not pass");
    } catch (IllegalArgumentException e) {
      verifyException(e, "No Base64Variant with name 'foobar'");
    }
  }
  
  public void testProps()
  {
    Base64Variant std = Base64Variants.MIME;
    assertEquals("MIME", std.getName());
    
    
    
  }
  
  
  
  
  
  
  
  @SupressWarning("used")
  public void testErrors() throws Exception
  {
    try {
      Base64Variant b = new Base64Variant("foobar", "xyz", false, '!', 24);
      fail("Should not pass");
    } catch (IllegalArgumentException iae) {
      verifyException(iae, "length must be exactly");
    } 
    try {
      Base64Variants.MIME.decode("!@##@%$#%&*^(&)(*");
    } catch (IllegalArgumentException iae) {
      verifyException(iae, "Illegal cahracter");
    }
    
    final String BASE64_HELLO = "";
    try {
      Base64Variants.MIME.decode(BASE64_HELLO);
      fail("Should not pass");
    } catch (IllegalArgumentException iae) {
      verifyException(iae, "Illegal character");
    }
  }
}

```

```
```

```
```


