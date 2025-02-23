# Работа с репозиториями
## 1 Создание репозитория
Для работы с данными `Spring Data JPA` предоставляет интерфейсы репозиториев. Чаще всего используется `JpaRepository`, который расширяет функционал базовых операций `CRUD`.

**PersonRepository.java**:
```java
// Класс наследуется от JpaRepository, который типизирован Person и Long, 
//  где, Person - это сущность
//       Long - это тип первичного ключа (может быть составным)
public interface PersonRepository extends JpaRepository<Person, Long> {
    // Метод для поиска пользователей по имени, реализуемый через механизм Query Derivation
    List<Person> findByName(String name);
}
```
## 2 Готовые методы репозиториев
При наследовании от `JpaRepository` становятся доступными такие методы, как:

`<S extends T> S save(S entity)` – сохранение или обновление сущности\
`<S extends T> List<S> saveAll(Iterable<S> entities)` - сохранить все переданные сущности\
`Optional<T> findById(ID id)` – поиск записи по идентификатору\
`boolean existsById(ID id)` - проверить наличие записи по id\
`List<T> findAll()` – получение всех записей\
`List<T> findAllById(Iterable<ID> ids)` – получение всех записей по указанным id\
`long count()` - получить количество записей в таблице
`void deleteById(ID id)` - удалить запись по id\
`void delete(T entity)` - удалить запись по сущности\
`void deleteAllById(Iterable<? extends ID> ids)` - удалить записи по указанным id\
`void deleteAll(Iterable<? extends T> entities)` - удалить записи переданных сущностей\
`void deleteAll()` - удалить все записи

## 3 Расширение репозитория
Помимо использования стандартных методов, можно добавлять свои собственные запросы:

### 3.1 [Query Derivation](2_queryDerivation)
Методы, имена которых строятся по соглашениям, например, `findByEmailContaining(String email)`

### 3.2 @Query
Использование аннотации для явного указания `JPQL` или нативного `SQL`-запроса.

**PersonRepository.java**:
```java
public interface PersonRepository extends JpaRepository<Person, Long> {
   // Пример метода с использованием @Query
   @Query("FROM User u WHERE u.email = ?1")
   Optional<User> findByEmail(String email);
}
```