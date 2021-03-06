# DescriptionSpring

# SpringMVC

Фреймворк Spring MVC обеспечивает архитектуру паттерна Model — View — Controller (Модель — Отображение (далее — Вид) — Контроллер) при помощи слабо связанных готовых компонентов. Паттерн MVC разделяет аспекты приложения (логику ввода, бизнес-логику и логику UI), обеспечивая при этом свободную связь между ними.

Model (Модель) инкапсулирует (объединяет) данные приложения, в целом они будут состоять из POJO («Старых добрых Java-объектов», или бинов).
View (Отображение, Вид) отвечает за отображение данных Модели, — как правило, генерируя HTML, которые мы видим в своём браузере.
Controller (Контроллер) обрабатывает запрос пользователя, создаёт соответствующую Модель и передаёт её для отображения в Вид.

DispatcherServlet

Вся логика работы Spring MVC построена вокруг DispatcherServlet, который принимает и обрабатывает все HTTP-запросы (из UI) и ответы на них. Рабочий процесс обработки запроса DispatcherServlet'ом проиллюстрирован на следующей диаграмме:


[![введите сюда описание изображения][1]][1]


  [1]: https://i.stack.imgur.com/rCKtx.png
  
  

Ниже приведена последовательность событий, соответствующая входящему HTTP-запросу:

После получения HTTP-запроса DispatcherServlet обращается к интерфейсу HandlerMapping, который определяет, какой Контроллер должен быть вызван, после чего, отправляет запрос в нужный Контроллер.
Контроллер принимает запрос и вызывает соответствующий служебный метод, основанный на GET или POST. Вызванный метод определяет данные Модели, основанные на определённой бизнес-логике и возвращает в DispatcherServlet имя Вида (View).
При помощи интерфейса ViewResolver DispatcherServlet определяет, какой Вид нужно использовать на основании полученного имени.
После того, как Вид (View) создан, DispatcherServlet отправляет данные Модели в виде атрибутов в Вид, который в конечном итоге отображается в браузере.

Все вышеупомянутые компоненты, а именно, HandlerMapping, Controller и ViewResolver, являются частями интерфейса WebApplicationContext extends ApplicationContext, с некоторыми дополнительными особенностями, необходимыми для создания web-приложений.

Определение Контроллера

DispatcherServlet отправляет запрос контроллерам для выполнения определённых функций. Аннотация @Controllerannotation указывает, что конкретный класс является контроллером. Аннотация @RequestMapping используется для мапинга (связывания) с URL для всего класса или для конкретного метода обработчика.

    @Controller
    @RequestMapping("/hello")
    public class HelloController { 
       @RequestMapping(method = RequestMethod.GET)
       public String printHello(ModelMap model) {
          model.addAttribute("message", "Hello Spring MVC Framework!");
          return "hello";
       }
    }
    
   Аннотация Controller определяет класс как Контроллер Spring MVC. В первом случае, @RequestMapping указывает, что все методы в данном Контроллере относятся к URL-адресу "/hello". Следующая аннотация @RequestMapping(method = RequestMethod.GET) используется для объявления метода printHello() как дефолтного метода для обработки HTTP-запросов GET (в данном Контроллере). Вы можете определить любой другой метод как обработчик всех POST-запросов по данному URL-адресу.

Вы можете написать вышеуказанный Контроллер по-другому, указав дополнительные атрибуты для аннотации @RequestMapping следующим образом:

    @Controller
    public class HelloController {
       @RequestMapping(value = "/hello", method = RequestMethod.GET)
       public String printHello(ModelMap model) {
          model.addAttribute("message", "Hello Spring MVC Framework!");
          return "hello";
       }
    }
    
Атрибут «value» указывает URL, с которым мы связываем данный метод (value = "/hello"), далее указывается, что этот метод будет обрабатывать GET-запросы (method = RequestMethod.GET). Также, нужно отметить важные моменты в отношении приведённого выше контроллера:

Вы определяете бизнес-логику внутри связанного таким образом служебного метода. Из него Вы можете вызывать любые другие методы.
Основываясь на заданной бизнес-логике, в рамках этого метода Вы создаёте Модель (Model). Вы можете добавлять аттрибуты Модели, которые будут добавлены в Вид (View). В примере выше мы создаём Модель с атрибутом «message».
Данный служебный метод возвращает имя Вида в виде строки String. В данном случае, запрашиваемый Вид имеет имя «hello».

# Controller 
(Слой представления) Аннотация для маркировки java класса, как класса контроллера. Данный класс представляет собой компонент, похожий на обычный сервлет (HttpServlet) (работающий с объектами HttpServletRequest и HttpServletResponse), но с расширенными возможностями от Spring Framework. (Взаимодействие с внешним миром)

# DTO

Data Transfer Object (DTO) — один из шаблонов проектирования, используется для передачи данных между подсистемами приложения. Data Transfer Object, в отличие от business object или data access object не должен содержать какого-либо поведения.

# DAO

data access object (DAO) — это объект, который предоставляет абстрактный интерфейс к какому-либо типу базы данных или механизму хранения. (Работа с БД).

# Entity

Entity (Сущность) — POJO-класс, связанный с БД с помощью аннотации (@Entity) или через XML. К такому классу предъявляются следующие требования:

Должен иметь пустой конструктор (public или protected)
Не может быть вложенным, интерфейсом или enum
Не может быть final и не может содержать final-полей/свойств
Должен содержать хотя бы одно @Id-поле

При этом entity может:

Содержать непустые конструкторы
Наследоваться и быть наследованным
Содержать другие методы и реализовывать интерфейсы
Entities могут быть связаны друг с другом (один-к-одному, один-ко-многим, многие-к-одному и многие-ко-многим).

# Repository
Это указывает на то, что класс определяет хранилище данных.

(Доменный слой) Аннотация показывает, что класс функционирует как репозиторий и требует наличия прозрачной трансляции исключений. Преимуществом трансляции исключений является то, что слой сервиса будет иметь дело с общей иерархией исключений от Spring (DataAccessException) вне зависимости от используемых технологий доступа к данным в слое данных.

# Service 

@Service - (Сервис-слой приложения) Аннотация, объявляющая, что этот класс представляет собой сервис – компонент сервис-слоя.
Сервис является подтипом класса @Component. Использование данной аннотации позволит искать бины-сервисы автоматически. (Бизнес-Логика)

# Other

@ResponseBody - Аннотация показывает что данный метод может возвращать кастомный объект в виде xml, json...

@RestController - Аннотация аккумулирует поведение двух аннотаций @Controller и @ResponseBody

@Autowired - Аннотация позволяет автоматически установить значение поля.

@RequestMapping - Аннотация используется для маппинга урл-адреса запроса на указанный метод или класс. Можно указывать конкретный HTTP-метод, который будет обрабатываться (GET/POST), передавать параметры запроса.

@PathVariable - Аннотация, которая показывает, что параметр метода должен быть связан с переменной из урл-адреса.
