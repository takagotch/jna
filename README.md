### jna
---
https://github.com/java-native-access/jna

```java
// test/com/sun/jna/DirectArgumentsWrappersMarshalTest.java

package com.sun.jna;

import java.util.Collections;
import java.util.HashMap;
import java.util.Map;

public class DirectArgumentsWrappersMarshalTest extends ArgumentsWrappersMarshalTest {
  
  public static class DirectTestLibrary implements TestLibrary {
    @Override
    public native Boolean returnBooleanArgument(Boolean arg);
    
    @Override
    public native Double checkDoubleArgumentAligment(Float i, Double j, Float i2, Double j2);
    
    static {
      class PrimitiveConverter implements FromNativeConverter, ToNativeConterter {
        private final Class nativeType;
        
        public PrimitiveConverter(Class nativeType) {
          this.nativeType = nativeType;
        }
        
        public Object fromNative(Object nativeValue, FromNativeContext context) {
          if(nativeValue == null) {
            return 0;
          } else {
            return nativeValue;
          }
        }
        
        public Class<?> nativeType() {
          return nativeType;
        }
        
        public Object toNative(Object value, ToNativeContext context) {
          if(value == null) {
            return 0;
          } else {
            return value;
          }
        }
      }
      final Map<Class,PrimitiveConverter> converters = new HashMap<Class,PrimitiveConverter>();
      converters.put(Boolean.class, new PrimitiveConverter(boolean.class));
      
      TypeMapper tm = new TypeMapper() {
        public FromNativeConverter getFromNativeConverter(Class<?> javaType) {
          return converters.get(javaType);
        }
        
        public ToNativeConverter getToNativeConverter(Class<?> javaType) {
          return converters.get(javaType);
        }
      };
      NativeLibrary testlib = NativeLibrary.getInstance("testlib",
        Collections.singletonMap(Library.OPTION_TYPE_MAPPER, tm));
      Native.registr(testlib);
    }
  }
  
  @Override
  protected void setUp() {
    lib = new DirectTestLibrary();
  }
  
  public static void main(java.lang.String[] argList) {
    juint.textui.TestRunner.run(DirectArgumentsWrappersMarshalTest.class);
  }
}
```

```
```

```
```


