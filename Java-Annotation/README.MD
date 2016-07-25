# Java 注解学习笔记

## 1. 定义注解

```
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Test {}
```

除了 @ 符号以外，`@Test`的定义很像一个空的接口。定义注解时，会需要一些**元注解**（meat-annotation），如`@Target`和`@Rentention`。`@Target`用来定义你的注解将应用于什么地方（例如是一个方法或者一个域）。`@Rentention`用来定义该注解在哪一个级别可用，在源代码中（**SOURCE**）、类文件中（**CLASS**）或者运行时（**RUNTIME**）。

在注解中，一般都会包含一些`元素`以表示某些值。当分析处理注解时，程序或者工具可以利用这些值。注解的元素看起来就像接口的方法，唯一的区别是你可以为其指定默认值。

没有`元素`的注解称为**标记注解**（marker annotation），例如上例的`@Test`。


定义**实例**：

```
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface UseCase {
	public int id();
	public String description() default "no description";
}
```

注解中的元素`id`和`description`定义类似方法的定义。

使用**实例**：

```

public class PasswordUtils {
	@UseCase(id = 47, description = "Passwords must contain at least one numeric")
	public boolean validatePassword(String password) {
		return (password.matches("\\w*\\d\\w*"));
	}

	@UseCase(id = 48)
	public String encryptPassword(String password) {
		return new StringBuilder(password).reverse().toString();
	}

	@UseCase(id = 49, description = "New passwords can't equal previously used ones")
	public boolean checkForNewPassword(List<String> 	prevPasswords, String password) {
		return !prevPasswords.contains(password);
	}
}

```

## 2. 元注解

Java目前只内置了3种**标准注解**（`@Override`, `@Deprecated`, `@SuppressWarnings`），以及4种**元注解**。**元注解**专职负责注解其他的注解：

|    注解名称    |     作用     |
|-----------------|-------------|
|    @Target    |	表示该注解可以用于什么地方。可能的ElementType参数包括：
					CONSTRUCTOR: 构造器的声明 
					FIELD: 域声明（包括enum实例）
					METHOD: 方法声明
					PACKAGE: 包声明
					PARAMETER: 参数声明
					TYPE: 类、接口（包括注解类型）或enum声明 |
|   @Retention  |   表示需要再什么级别保存该注解信息。可选的Retention参数包括：
					SOURCE:	 注解将被编译器丢弃
					CLASS: 注解在calss文件中可用，但会被VM丢弃
					RUNTIOME: VM将在运行期也保留注解，因此可以通过反射机制读取注解的信息。 |
|   @Documented |	将此注解包含在Javadoc中    |
|   @Inherited  |	允许子类继承父类中的注解   |

	
## 3. 编写注解处理器
如果没有用来读取注解的工具，那注解也不会比注释更有用。使用注解的过程中，很重要的一个部分就是创建与使用`注解处理器`。
下面是一个非常简单的`注解处理器`，我们将用它来读取PasswordUtils类，并使用反射机制查找@UseCase标记。我们为其提供了一组id值，然后它会列出在PasswordUtils中找到的用例，以及缺失的用例。

```
public class UseCaseTracker {
	public static void trackUseCases(List<Integer> useCases, Class<?> cl) {
		for (Method m : cl.getDeclaredMethods()) {
			UseCase uc = m.getAnnotation(UseCase.class);
			if(uc != null){
				System.out.println("Found Use Case: " + uc.id() + " " + uc.description());
				useCases.remove(new Integer(uc.id()));
			}
		}
		for (int i : useCases) {
			System.out.println("Warning: Missing use case-" + i);
		}
	}

	public static void main(String[] args) {
		List<Integer> useCases = new ArrayList<Integer>();
		Collections.addAll(useCases, 47, 48, 49, 50);
		trackUseCases(useCases, PasswordUtils.class);
	}
}

```

**Output**

```

Found Use Case: 47 Passwords must contain at least one numeric
Fonud Use Case: 48 no description
Found Use Case: 49 New passwords can't equal previously used ones
Warning : Missing use Case-50


```

## 4. 注解元素

标签**@UseCase**由**UseCase.java**定义，其中包含int元素id，以及一个String元素description。注解元素可用的类型如下所示：
* 所有基本类型（int，float，boolean等）
* String
* Class
* enum
* Annotation
* 以上类型的数组

