TP : Spring Boot et Thymeleaf - Application Web CRUD
Ce projet est un exemple d'application Web CRUD (Create, Read, Update, Delete) développée avec Spring Boot et Thymeleaf. Il permet la gestion d'utilisateurs à travers une interface web simple.

Objectifs
L'objectif de ce TP est d'apprendre à :

Configurer un projet Spring Boot avec des dépendances Maven.
Créer une couche DAO à l'aide de Spring Data JPA.
Utiliser Thymeleaf pour la présentation.
Implémenter des fonctionnalités CRUD sur des entités JPA.
Prérequis
Java 8 ou version ultérieure
Maven
MySQL
IDE avec prise en charge de Spring Boot (IntelliJ, Eclipse, etc.)
Étapes de mise en place
1. Configuration des dépendances Maven
Le fichier pom.xml inclut toutes les dépendances nécessaires, telles que :

Spring Boot Starter Web
Spring Boot Starter Data JPA
Spring Boot Starter Thymeleaf
MySQL Connector Java
Voici un extrait du fichier pom.xml :


<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
2. Configuration de application.properties
Le fichier de configuration application.properties permet de définir les paramètres de la base de données MySQL :

properties
Copier le code
server.port=8080
spring.datasource.url=jdbc:mysql://localhost:3306/thymeleaf?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=update
3. Implémentation de la couche domaine
La classe User représente l'entité utilisateur :


@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private long id;
    @NotBlank(message = "Name is mandatory")
    private String name;
    @NotBlank(message = "Email is mandatory")
    private String email;
    
    // Getters et setters
}
4. Création de la couche repository
L'interface UserRepository étend CrudRepository pour fournir des opérations CRUD sur l'entité User :


@Repository
public interface UserRepository extends CrudRepository<User, Long> {}
5. Implémentation de la couche contrôleur
Le contrôleur UserController gère les requêtes HTTP et les opérations CRUD :


@Controller
public class UserController {
    @Autowired
    private UserRepository userRepository;

    @GetMapping("/signup")
    public String showSignUpForm(User user) {
        return "add-user";
    }

    @PostMapping("/adduser")
    public String addUser(@Valid User user, BindingResult result, Model model) {
        if (result.hasErrors()) {
            return "add-user";
        }
        userRepository.save(user);
        model.addAttribute("users", userRepository.findAll());
        return "index";
    }
}
6. Création de la couche présentation (Vue)
Les templates Thymeleaf sont utilisés pour afficher et interagir avec les données des utilisateurs. Exemples de fichiers :

add-user.html : Formulaire d'ajout d'utilisateur.
update-user.html : Formulaire de mise à jour.
index.html : Liste des utilisateurs avec les actions d'édition et de suppression.
7. Exécution de l'application
Exécutez l'application en utilisant la méthode main dans la classe DemothymeleafApplication :



@SpringBootApplication
public class DemothymeleafApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemothymeleafApplication.class, args);
    }
}
Une fois démarrée, ouvrez un navigateur à l'adresse http://localhost:8080 pour accéder à l'application.

Conclusion
Ce projet montre comment configurer une application Spring Boot pour créer une application CRUD basique avec Thymeleaf. Vous pouvez étendre cet exemple pour ajouter plus de fonctionnalités et personnaliser l'interface utilisateur.# ilham999-byte-Spring-Boot-et-Thymeleaf
