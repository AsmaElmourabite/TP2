# TP2 - Application Spring avec Injection de Dépendances

##  Description
Ce projet est une application Java développée avec le framework Spring, mettant en œuvre les concepts d'**Inversion de Contrôle (IoC)** et d'**Injection de Dépendances (DI)** via des annotations. L'application suit une architecture en couches avec des composants DAO, Métier et Présentation.

##  Objectifs Pédagogiques
- Comprendre l'Inversion de Contrôle (IoC) avec Spring
- Maîtriser l'Injection de Dépendances via annotations
- Implémenter une architecture en couches (DAO, Métier, Présentation)
- Configurer Spring avec des annotations
- Gérer les dépendances entre composants

##  Architecture du Projet
```
TP2/
├── src/
│   ├── main/
│   │   └── java/
│   │       ├── dao/
│   │       │   ├── IDao.java (Interface)
│   │       │   ├── DaoImpl.java (Implémentation 1)
│   │       │   └── DaoImpl2.java (Implémentation 2)
│   │       ├── metier/
│   │       │   ├── IMetier.java (Interface)
│   │       │   └── MetierImpl.java (Implémentation)
│   │       └── presentation/
│   │           └── Presentation2.java (Classe principale)
│   └── resources/
│       └── applicationContext.xml (Configuration Spring)
├── target/ (Dossier de compilation)
├── cible/ (Dossier de sortie)
└── pom.xml
```

##  Prérequis
- **Eclipse IDE for Java Developers** ou **IntelliJ IDEA**
- **JDK 11** ou version supérieure
- **Maven** 3.6+ pour la gestion des dépendances
- **Spring Framework** 5.x

##  Dépendances Maven

### Fichier `pom.xml` :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>tp2-spring</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>
    
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <spring.version>5.3.20</spring.version>
    </properties>
    
    <dependencies>
        <!-- Spring Context -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        
        <!-- Spring Core -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        
        <!-- Spring Beans -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>11</source>
                    <target>11</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

## Installation et Exécution

### 1. Cloner le dépôt
```bash
git clone https://github.com/AsmaElmourabite/TP2.git
cd TP2
```

### 2. Compiler avec Maven
```bash
mvn clean compile
```

### 3. Exécuter l'application
```bash
mvn exec:java -Dexec.mainClass="presentation.Presentation2"
```

### 4. Pour créer le package
```bash
mvn clean package
```

## Structure des Composants

### Couche DAO (Data Access Object)
- **IDao.java** : Interface définissant les opérations d'accès aux données
- **DaoImpl.java** : Première implémentation concrète
- **DaoImpl2.java** : Deuxième implémentation (alternative)

### Couche Métier (Business Logic)
- **IMetier.java** : Interface définissant les opérations métier
- **MetierImpl.java** : Implémentation de la logique métier

### Couche Présentation
- **Presentation2.java** : Classe principale qui utilise les composants métier

## 🔧 Configuration Spring

### Configuration par Annotations
```java
// Dans DaoImpl.java
@Repository("dao1")
public class DaoImpl implements IDao {
    // Implémentation des méthodes DAO
}

// Dans MetierImpl.java
@Service
public class MetierImpl implements IMetier {
    @Autowired
    @Qualifier("dao1") // ou "dao2" pour l'autre implémentation
    private IDao dao;
    
    // Implémentation des méthodes métier
}
```

### Configuration XML (optionnelle)
```xml
<!-- src/main/resources/applicationContext.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="dao, metier, presentation"/>
    
    <!-- Définitions de beans alternatives -->
    <bean id="dao1" class="dao.DaoImpl"/>
    <bean id="dao2" class="dao.DaoImpl2"/>
    
    <bean id="metier" class="metier.MetierImpl">
        <property name="dao" ref="dao1"/>
    </bean>
</beans>
```

##  Exemple de Code

### Interface IDao
```java
package dao;

public interface IDao {
    double getData();
}
```

### Implémentation DaoImpl
```java
package dao;

import org.springframework.stereotype.Repository;

@Repository("dao1")
public class DaoImpl implements IDao {
    @Override
    public double getData() {
        // Implémentation réelle
        return Math.random() * 100;
    }
}
```

### Interface IMetier
```java
package metier;

public interface IMetier {
    double calcul();
}
```

### Implémentation MetierImpl
```java
package metier;

import dao.IDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class MetierImpl implements IMetier {
    
    @Autowired
    private IDao dao;
    
    @Override
    public double calcul() {
        double data = dao.getData();
        return data * 10;
    }
    
    // Setter pour l'injection (optionnel avec @Autowired)
    public void setDao(IDao dao) {
        this.dao = dao;
    }
}
```

### Classe Principale Presentation2
```java
package presentation;

import metier.IMetier;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Presentation2 {
    public static void main(String[] args) {
        // Configuration par annotations
        ApplicationContext context = 
            new AnnotationConfigApplicationContext("dao", "metier");
        
        // Récupération du bean métier
        IMetier metier = context.getBean(IMetier.class);
        
        // Utilisation
        double result = metier.calcul();
        System.out.println("Résultat du calcul : " + result);
    }
}
```

## 🎮 Utilisation et Tests

### Tester avec différentes implémentations DAO
1. Modifiez l'annotation `@Qualifier` dans `MetierImpl.java`
2. Passez de `"dao1"` à `"dao2"` pour tester l'autre implémentation
3. Relancez l'application pour voir la différence

### Exécution avec configuration XML
```java
ApplicationContext context = 
    new ClassPathXmlApplicationContext("applicationContext.xml");
```

## 📚 Concepts Spring Implémentés

- ✅ **Inversion de Contrôle (IoC)** : Le conteneur Spring gère le cycle de vie des objets
- ✅ **Injection de Dépendances** : Via `@Autowired`, `@Qualifier` et setters
- ✅ **Annotations Spring** : `@Component`, `@Service`, `@Repository`, `@Autowired`
- ✅ **Couplage faible** : Programmation orientée interface
- ✅ **Scan de composants** : Détection automatique des beans
- ✅ **Configuration mixte** : Annotations + XML possible

## 🔍 Bonnes Pratiques Appliquées

1. **Séparation des concerns** : Architecture en 3 couches
2. **Interfaces** : Abstraction entre les couches
3. **Annotations appropriées** : `@Service` pour la couche métier, `@Repository` pour DAO
4. **Injection par constructeur** : Alternative recommandée
5. **Packaging cohérent** : Organisation par fonctionnalité

## 🐛 Dépannage

### Problèmes courants :
- **Bean non trouvé** : Vérifiez le scan des packages
- **Dépendance circulaire** : Utilisez `@Lazy` ou restructurez
- **Conflit de beans** : Utilisez `@Qualifier` pour préciser

### Solutions :
```bash
# Nettoyer et recompiler
mvn clean compile

# Vérifier les dépendances
mvn dependency:tree

# Exécuter avec debug
mvn exec:java -Dexec.mainClass="presentation.Presentation2" -X
```

 jour : ${date}*
