## 테스트 코드에서 auto_increment 초기화 이슈

```java
@DataJpaTest // DB와 관련된 컴포넌트만 메모리에 로딩(컨트롤러, 서비스 제외)
public class BookRepositoryTest {

    @Autowired
    private BookRepository bookRepository;

    @BeforeEach
    void setUp() {
        String title = "junit5";
        String author = "저자";

        Book book = Book.builder()
                .title(title)
                .author(author)
                .build();

        Book savedBook = bookRepository.save(book);
    }

    @Test
    @DisplayName("책 저장")
    void saveBook() throws Exception {
        // given
        String title = "junit5";
        String author = "저자";

        Book book = Book.builder()
                .title(title)
                .author(author)
                .build();

        // when
        Book savedBook = bookRepository.save(book);

        // then
        assertThat(title).isEqualTo(savedBook.getTitle());
        assertThat(author).isEqualTo(savedBook.getAuthor());
    }

    @Test
    @DisplayName("책 목록 조회")
    void findBookList() throws Exception {
        // given
        String title = "junit5";
        String author = "저자";

        Book book = Book.builder()
                .title(title)
                .author(author)
                .build();

        // when
        List<Book> books = bookRepository.findAll();

        // then
        assertThat(book.getTitle()).isEqualTo(books.get(0).getTitle());
        assertThat(book.getAuthor() ).isEqualTo(books.get(0).getAuthor());
    }

    @Test
    @DisplayName("책 단건 조회")
    void findBook() throws Exception {
        // given
        String title = "junit5";
        String author = "저자";

        Book book = Book.builder()
                .title(title)
                .author(author)
                .build();

        // when
        Book foundBook = bookRepository.findById(1L).get();

        // then
        assertThat(book.getTitle()).isEqualTo(foundBook.getTitle());
        assertThat(book.getAuthor() ).isEqualTo(foundBook.getAuthor());
    }

}
```
findBook 메서드를 실행하면 테스트가 실패한다.  
bookRepository.findById(1L).get(); 이 부분에서 책을 찾을 수 없기 때문이다.
각 테스트 메서드가 종료되면 저장된 book이 초기화 되지면 AUTO_INCREMENT 값은 초기화가 되지 않기 때문에 발생하는 이슈이다.

## 해결 방법
@AfterEach 어노테이션을 사용하여 각 테스트 메서드가 종료될 때마다 Native Query를 사용하여 AUTO_INCREMENT 값을 초기화 해준다.
```java
    @AfterEach
    void initAutoIncrement() {
        em.createNativeQuery("ALTER TABLE BOOK ALTER COLUMN `id` RESTART WITH 1").executeUpdate();
    }
```