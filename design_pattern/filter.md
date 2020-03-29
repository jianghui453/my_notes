# 过滤器模式

一个对象的不同条件进行过滤，这时候就可以通过不同的过滤器进行组合。

```
public class Person {
    private String name;//姓名
    private String gender;//性别
    
    public Person(String name, String gender) {
        this.name = name;
        this.gender = gender;
    }
    
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getGender() {
        return gender;
    }
    public void setGender(String gender) {
        this.gender = gender;
    }
    @Override
    public String toString() {
        return "Person [name=" + name + ", gender=" + gender + "]";
    }
}

public interface IFilter {
    public List<Person> filter(List<Person> persons);
}

public class GirlFilter implements IFilter {
    @Override
    public List<Person> filter(List<Person> persons) {
         return persons.stream().filter(person ->
         person.getGender().equals("男")
     ).collect(Collectors.toList());
 }
}
​
public class BoyFilter implements IFilter {
    @Override
    public List<Person> filter(List<Person> persons) {
         return persons.stream().filter(person ->
         person.getGender().equals("男")
     ).collect(Collectors.toList());
 }
}

public class Test {
    public static void main(String[] args) {
        List<Person> persons = new LinkedList<>();
         for (int i = 0; i < 10 ; i++) {
                if (i<5) {
                    persons.add(new Person(String.valueOf(i), "男"));
                }else {
                    persons.add(new Person(String.valueOf(i), "女"));
                }
            }
         IFilter boyFilter = new BoyFilter();
        //筛选出男同胞们
         persons = boyFilter.filter(persons);
         persons.forEach(person->System.out.println(person.toString()));
    }
}
```