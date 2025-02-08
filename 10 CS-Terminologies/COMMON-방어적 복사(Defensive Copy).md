---
created: 2025-02-08 16:23
tags:
  - cs-terminology
last_modified: 2025-02-08T16:51:00
---
## 방어적 복사(Defensive Copy)란?
> 객체의 내부 상태를 외부로부터 보호하기 위해 사용되는 기법.

- 객체의 필드나 반환 값을 외부에서 변경할 수 없도록 복사본을 제공하는 기법이다.
- 외부에서 복사본을 수정하더라도 원본 객체의 상태는 변경되지 않는다.
- 불변 객체의 내부 상태를 안전하게 유지하거나, 원본 데이터의 무결성 보장을 위해 사용된다.

#### 예시
```java
class Person {
	// `final`을 사용하면 객체가 생성된 후 해당 필드의 참조값을 변경할 수 없다.
	// `name`과 `hobbies` 필드는 생성자에서만 한 번 할당되고 이후 변경 불가능하다.
    private final String name;
    private final List<String> hobbies;

	// 생성자에서 방어적 복사
    public Person(String name, List<String> hobbies) {
        this.name = name;
        // 방어적 복사 (만약 `this.hobbies = hobbies;`처럼 직접 할당하면, 외부에서 `hobbies` 리스트를 변경하면 `Person` 내부의 `hobbies`도 함께 변경된다)
        this.hobbies = new ArrayList<>(hobbies);  // 새로운 리스트를 생성하면, 원본 리스트의 영향을 받지 않는다.
    }

    public String getName() {
        return name; // `name`은 `String` (불변 객체)이므로 방어적 복사가 필요 없다.
    }

    public List<String> getHobbies() {
        return new ArrayList<>(hobbies); // 방어적 복사 (외부에서 반환된 리스트를 변경해도 원본 리스트가 변경되지 않도록 보호한다)
    }
}

public class DefensiveCopyExample {
    public static void main(String[] args) {
	    // `hobbies`라는 리스트를 생성하고, `"Reading"`과 `"Gaming"`을 추가한다.
        List<String> hobbies = new ArrayList<>();
        hobbies.add("Reading");
        hobbies.add("Gaming");

        Person person = new Person("Alice", hobbies);
        System.out.println(person.getHobbies()); // [Reading, Gaming]

        hobbies.add("Swimming"); // 원본 리스트 수정 시도
        System.out.println(person.getHobbies()); // [Reading, Gaming] (변경되지 않음 -> 방어적 복사에 의해)
    }
}
```

#### 다른 방법
- `Collections.unmodifiableList()`를 사용하면, 
	- 반환된 리스트를 수정하려고 할 때 `UnsupportedOperationException`이 발생한다.
```java
public List<String> getHobbies() {
    return Collections.unmodifiableList(hobbies);
}
```
---
> **참고**
> - ChatGPT