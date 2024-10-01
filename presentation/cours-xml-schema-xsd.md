# Cours : Introduction à XML-Schema (XSD)

## Objectifs du cours
À la fin de ce cours, vous serez capables de :
- Comprendre les concepts de base de XML-Schema (XSD)
- Créer un fichier XML conforme à un schéma XSD
- Utiliser les types de données et les contraintes de validation
- Comprendre et utiliser les espaces de nommage en XML et XSD
- Créer des énumérations et utiliser des expressions régulières

---

## Introduction à XML-Schema (XSD)

**XML-Schema** (ou **XSD** pour *XML Schema Definition*) est un langage qui permet de définir la structure, le contenu et les types de données d’un fichier XML. Il joue un rôle similaire à une grammaire pour les fichiers XML, garantissant que les documents XML sont bien formés et conformes à une structure définie.

---

### Pourquoi utiliser XML-Schema ?
- Valider que les données XML sont conformes à une structure attendue.
- Définir des types de données spécifiques pour les éléments XML.
- Imposer des contraintes sur la longueur, le format et la valeur des données.

---

### Exemple simple d’un schéma XML

```xml
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:element name="person" type="xsd:string"/>
</xsd:schema>
```

Ce schéma décrit un élément XML appelé person qui doit contenir une chaîne de caractères.

---

## Structure d'un fichier XSD

Les éléments principaux d’un fichier XSD :
- `xsd:element` : définit un élément XML.
- `xsd:complexType` : définit une structure complexe d’éléments.complexe d’éléments.
- `xsd:simpleType` : définit des types simples, comme des chaînes de caractères ou des nombres.
- `xsd:sequence` : définit l’ordre des éléments dans un élément complexe.

---

Exemple :

```xml
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:element name="person">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element name="firstname" type="xsd:string"/>
                <xsd:element name="lastname" type="xsd:string"/>
                <xsd:element name="age" type="xsd:integer"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
</xsd:schema>
```

Dans cet exemple, l’élément person est composé de trois sous-éléments : firstname, lastname et age.

---
## Types de données dans XSD

XML-Schema permet de définir plusieurs types de données pour les éléments. Voici quelques types courants :

| Type de données | Description                  | Exemple de valeur       |
|-----------------|------------------------------|-------------------------|
| `xsd:string`    | Chaîne de caractères         | "Bonjour"               |
| `xsd:integer`   | Nombre entier                | 42                      |
| `xsd:decimal`   | Nombre décimal               | 3.14                    |
| `xsd:boolean`   | Booléen (true/false)         | true                    |
| `xsd:date`      | Date (YYYY-MM-DD)            | 2023-10-01              |

---
Exemple

```xml
<xsd:element name="product">
    <xsd:complexType>
        <xsd:sequence>
            <xsd:element name="name" type="xsd:string"/>
            <xsd:element name="price" type="xsd:decimal"/>
            <xsd:element name="available" type="xsd:boolean"/>
        </xsd:sequence>
    </xsd:complexType>
</xsd:element>
```

---
## Contraintes de validation dans XSD

XSD permet d’ajouter des contraintes sur les données pour les valider.
Exemples de contraintes :


| Contrainte                | Élément XSD               | Exemple                                                                 |
|---------------------------|---------------------------|-------------------------------------------------------------------------|
| Longueur minimale         | `<xsd:minLength>`         | `<xsd:minLength value="5"/>`                                            |
| Longueur maximale         | `<xsd:maxLength>`         | `<xsd:maxLength value="20"/>`                                           |
| Valeur numérique minimale | `<xsd:minInclusive>`      | `<xsd:minInclusive value="10"/>`                                        |
| Valeur numérique maximale | `<xsd:maxInclusive>`      | `<xsd:maxInclusive value="100"/>`                                       |
| Expression régulière      | `<xsd:pattern>`           | `<xsd:pattern value="[A-Z\s]+"/>`                                       |

    Ces éléments permettent de définir des contraintes de validation pour les éléments XML dans un schéma XSD.

---
Exemple : Contraintes de validation

```xml

<xsd:element name="username">
    <xsd:simpleType>
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="5"/>
            <xsd:maxLength value="20"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:element>
```

Cet exemple impose que l'élément username ait une longueur comprise entre 5 et 20 caractères.

---
## Expressions régulières dans XSD

XSD permet d'utiliser des expressions régulières pour valider des formats spécifiques, comme les adresses e-mail ou les formats de texte. L'élément xsd est utilisé pour cela.

Exemple 1 : Valider un titre en majuscules

```xml
<xsd:element name="title">
    <xsd:simpleType>
        <xsd:restriction base="xsd:string">
            <xsd:pattern value="[A-Z\s]+"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:element>
```

Ce schéma impose que l'élément title soit composé uniquement de lettres majuscules et d'espaces.

---

Exemple 2 : Valider une adresse e-mail

```xml
<xsd:element name="email">
    <xsd:simpleType>
        <xsd:restriction base="xsd:string">
            <xsd:pattern value="[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:element>
```

Ce schéma impose un format d'e-mail simple, comme user@example.com.

---
## Réutilisation de types dans XSD

Il est possible de définir des types réutilisables dans un schéma XSD, ce qui permet de maintenir la cohérence et de réduire la redondance. Pour cela, on utilise les éléments `<xsd:simpleType>` ou `<xsd:complexType>` avec un attribut `name` pour définir un type global.

---
### Exemple de type simple réutilisable

```xml
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!-- Définition d'un type simple réutilisable -->
    <xsd:simpleType name="UppercaseNonEmptyString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
            <xsd:pattern value="[A-Z\s]+"/>
        </xsd:restriction>
    </xsd:simpleType>

    <!-- Utilisation du type réutilisable -->
    <xsd:element name="firstname" type="UppercaseNonEmptyString"/>
    <xsd:element name="lastname" type="UppercaseNonEmptyString"/>
</xsd:schema>
```

Dans cet exemple, le type `UppercaseNonEmptyString` impose que la chaîne soit non vide et composée uniquement de lettres majuscules et d'espaces. Ce type est ensuite réutilisé pour les éléments `firstname` et `lastname`.

---
### Exemple de type complexe réutilisable

```xml
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!-- Définition d'un type complexe réutilisable -->
    <xsd:complexType name="AddressType">
        <xsd:sequence>
            <xsd:element name="street" type="xsd:string"/>
            <xsd:element name="city" type="xsd:string"/>
            <xsd:element name="zipcode" type="xsd:string"/>
        </xsd:sequence>
    </xsd:complexType>

    <!-- Utilisation du type réutilisable -->
    <xsd:element name="billingAddress" type="AddressType"/>
    <xsd:element name="shippingAddress" type="AddressType"/>
</xsd:schema>
```

Dans cet exemple, le type `AddressType` est défini une fois et réutilisé pour les éléments `billingAddress` et `shippingAddress`.

Ces techniques permettent de créer des schémas XML plus modulaires et maintenables.

---
## Les énumérations dans XSD

Les énumérations permettent de restreindre les valeurs possibles d'un élément à une liste définie.
Exemple d’énumération

```xml
<xsd:element name="genre">
    <xsd:simpleType>
        <xsd:restriction base="xsd:string">
            <xsd:enumeration value="Action"/>
            <xsd:enumeration value="RPG"/>
            <xsd:enumeration value="Sport"/>
            <xsd:enumeration value="Aventure"/>
            <xsd:enumeration value="Stratégie"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:element>
```

Ici, l'élément genre doit être l'une des valeurs spécifiées dans la liste : Action, RPG, Sport, Aventure, ou Stratégie.

---
## Les espaces de nommage (namespaces)

Un espace de nommage (namespace) est une manière d’éviter les conflits de noms dans les documents XML. Il permet de distinguer différents éléments qui pourraient avoir le même nom, en les associant à des URI uniques.
Définition d’un espace de nommage

L’attribut `xmlns` est utilisé pour déclarer un espace de nommage dans un document XML ou XSD.

Exemple d’utilisation dans XML

```xml
<person xmlns="http://example.com/person">
    <firstname>John</firstname>
    <lastname>Doe</lastname>
</person>
```

---
## Prefixes d’espace de nommage

Il est possible d’utiliser des préfixes pour les espaces de nommage afin de les distinguer dans un document XML.

Exemple d’utilisation de préfixes

```xml
<p:person xmlns:p="http://example.com/person">
    <p:firstname>John</p:firstname>
    <p:lastname>Doe</p:lastname>
</p:person>
```

Il est possible de mixer les espaces de nommage avec et sans préfixes dans un document XML.

---
## Portée des espaces de nommage

La portée des espaces de nommage est limitée à l’élément où ils sont déclarés et à ses sous-éléments.

```xml
<p:person xmlns:p="http://example.com/person">
    <p:firstname>John</p:firstname>
    <p:lastname>Doe</p:lastname>
    <address xmlns="http://example.com/address">
        <street>Main Street</street>
        <city>City</city>
    </address>
</p:person>
```

Dans cet exemple, l’espace de nommage `http://example.com/address` est utilisé pour les éléments de l’élément address, tandis que l’espace de nommage `http://example.com/person` est utilisé pour les éléments de l’élément person.

---
## Espaces de nommage dans XSD

Dans les schémas XSD, il est également possible d’utiliser des espaces de nommage pour éviter les conflits de définition.

```xml
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"
           xmlns:person="http://example.com/person"
           targetNamespace="http://example.com/person"
           elementFormDefault="qualified">
    <xsd:element name="person">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element name="firstname" type="xsd:string"/>
                <xsd:element name="lastname" type="xsd:string"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
</xsd:schema>
```

Points importants :
- `xmlns:xsd="http://www.w3.org/2001/XMLSchema"` : espace de nommage pour les éléments XSD.
- `targetNamespace` : indique l’espace de nommage de la définition du schéma.
- `elementFormDefault="qualified"` : impose que tous les éléments utilisent un espace de nommage.

---
## Exercice pratique : Validation de jeux vidéo

Créez un fichier XSD qui valide un fichier XML représentant une collection de jeux vidéo.

Chaque jeu vidéo contient les informations suivantes :
- `title` : Le titre du jeu (chaîne de caractères en majuscules)
- `genre` : Le genre du jeu (chaîne parmi les valeurs "Action", "RPG", "Sport", "Aventure", "Stratégie")
- `releaseYear` : L’année de sortie (nombre entier compris entre 1980 et l’année actuelle)
- `platform` : La plateforme (chaîne parmi les valeurs "PC", "PlayStation", "Xbox", "Nintendo", "Mobile")
- `price` : Le prix (nombre décimal positif)

L'espace de nommage du schéma doit être `http://xml.learn.com/games`.

---
Exemple de fichier XML (attention l'espace de nommage n'est pas encore défini, volontairement) :

```xml
<games>
    <game>
        <title>THE LEGEND OF ZELDA</title>
        <genre>Aventure</genre>
        <releaseYear>1986</releaseYear>
        <platform>NES</platform>
        <price>59.99</price>
    </game>
    <game>
        <title>FINAL FANTASY VII</title>
        <genre>RPG</genre>
        <releaseYear>1997</releaseYear>
        <platform>PLAYSTATION</platform>
        <price>49.99</price>
    </game>
</games>
```

Utiliser ce site : https://www.liquid-technologies.com/online-xsd-validator pour valider votre fichier XML.



---
## Conclusion

Dans ce cours, nous avons vu les concepts de base de XML-Schema (XSD), notamment la structure des fichiers XSD, les types de données, les contraintes de validation, les énumérations, et les expressions régulières. Vous êtes maintenant prêts à utiliser ces notions pour valider des fichiers XML à l'aide de XSD.

Accès au cours : [Introduction à XML-Schema (XSD)](https://hackmd.io/@I1LI1qnoQ4-EuYBUVTfdSA/SJDCQPKCR)
