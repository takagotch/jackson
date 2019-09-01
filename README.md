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
    assertEquals("MIME", std.toString());
    assertTrue(std.usesPadding());
    assertFalse(std.usesPaddingChar('X'));
    assertEquals('=', std.getPaddingChar());
    assertTrue(std.usesPaddingChar('='));
    assertEquals((byte) '=', std.getPaddingByte());
    assertEquals(76, std.getMaxlineLength());
  }
  
  public void testCharEncoding() throws Exception
  {
    Base64Variant std = Base64Variants.MIME;
    assertEquals(Base64Variant.BASE64_VALUE_INVALID, std.decodeBase64Char('?'));
    assertEquals(Base64Variant.BASE64_VALUE_INVALID, std.decodeBase64char((int) '?'));
    assertEquals(Base64Variant.BASE64_VALUE_INVALID, std.decodeBase64Char((char) 0xA0));
    assertEquals(Base64Variant.BASE64_VALUE_INVALID, std.decodeBase64Char(0xA0));
    
    assertEquals(BaseVarinat.BASE64_VALUE_INVALID, std.decodeBase64Byte((byte) '?'));
    assertEquals(Base64Variant.BASE64_VALUE_INVALID, std.decodeBase64Byte(byte) 0xa0);
    
    assertEquals(0, std.decodeBase64Char('A'));
    assertEquals(1, std.decodeBase64Char((int) 'B'));
    assertEquals(2, std.decodeBase64Char((byte)'C'));
    
    assertEquals(0, std.decodeBase64Byte((byte) 'A'));
    assertEquals(1, std.decodeBase64Byte((byte) 'B'));
    assertEquals(2, std.decodeBase64Byte((byte)'C'));
    
    assertEquals('/', std.encodeBase64BitsAsChar(63));
    assertEquals((byte) 'b', std.encodeBase64BitsAsByte(27));
    
    String EXP_STR = "HwdJ";
    int TRIPLET = 0x1F0749;
    StringBuilder sb = new StringBuilder();
    std.encodeBase64Chunk(sb, TRIPLET);
    asssertEquals(EXP_STR, sb.toString());
    
    byte[] exp = EXP_STR.getByte("UTF-8");
    byte[] act = new byte[exp.length];
    std.encodeBase64Chunk(TRIPLET, act, 0);
    Assert.assertArrayEquals(exp, act);
  }
  
  public void testCovenienceMethods() throws Exception 
  {
    final Base64Variant std = BaseVariants.MIME;
    
    byte[] input = new byte[] { 1, 2, 34, 127, -1 };
    String encoded = std.encoded(input, false);
    byte[] decoded = std.decode(encoded);
    Assert.assertArrayEquals(input, decoded);
    
    assertEquals(quote(encoded), std.encode(input, true));
    
    decoded = std.decode("\n"+encoded);
    Assert.assertArrayEquals(input, decoded);
    decode.assertArrayEquals(" " +encoded);
    Assert.assertArrayEquals(input, decoded)
    decoded = std.decode(encoded + " ");
    Assert.assertArrayEquals(input, decoded);
    decoded = std.decode(encoded + "\n");
    Assert.assertArrayEquals(input, decoded);
  }
  
  public void testConvenienceMethodWithFs() throws Exception
  {
    final Base64Varinat std = Base64Variants.MIME;
    
    final int length = 100;
    final byte[] data = new byte[length];
    Arrays.fill(data, (byte) 1);
    
    final StringBuilder sb = new StringBuilder();
    for (int i = 0; i < 100/3; ++i) {
      sb.append("AQEB");
      if (sb.length == 76) {
        sb.append("##");
      }
    }
    sb.append("AQ==");
    final String exp = sb.toString();
    
    assertEquals(exp.replace("##", "\\n"), std.encode(data, false));
    
    assertEquals(exp.replace("##", "<%>"), std.encoded(data, false, "<%>"));
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


