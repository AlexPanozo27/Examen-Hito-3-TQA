# Escenarios Gherkin (BDD) — SauceDemo

```gherkin
Feature: Inicio de sesión en Swag Labs
  Como cliente de la tienda
  Quiero iniciar sesión con mi usuario
  Para poder ver y comprar productos

  Background:
    Given que estoy en la página de login "https://www.saucedemo.com/"

  Scenario: Acceso exitoso con credenciales válidas
    When ingreso el usuario "standard_user"
    And ingreso la contraseña "secret_sauce"
    And presiono el botón "Login"
    Then debería ser redirigido a la página "/inventory.html"
    And debería ver el título "Products"

  Scenario: Acceso denegado a usuario bloqueado
    When ingreso el usuario "locked_out_user"
    And ingreso la contraseña "secret_sauce"
    And presiono el botón "Login"
    Then debería permanecer en la página de login
    And debería ver el mensaje "Epic sadface: Sorry, this user has been locked out."
```

```gherkin
Feature: Checkout de compra
  Como cliente con productos en el carrito
  Quiero completar mis datos de envío
  Para finalizar mi compra

  Background:
    Given que inicié sesión como "standard_user"
    And tengo el producto "Sauce Labs Backpack" en el carrito
    And estoy en la página "Checkout: Your Information"

  Scenario: Compra completada con datos válidos
    When ingreso "Marcos" en First Name
    And ingreso "Perez" en Last Name
    And ingreso "0000" en Zip/Postal Code
    And presiono "Continue"
    Then debería ver el total "Total: $32.39"
    When presiono "Finish"
    Then debería ver el mensaje "Thank you for your order!"

  Scenario: Nombre obligatorio vacío
    When dejo vacío el campo First Name
    And ingreso "Perez" en Last Name
    And ingreso "0000" en Zip/Postal Code
    And presiono "Continue"
    Then debería ver el mensaje "Error: First Name is required"
    But no debería avanzar al resumen de la compra
```

> Gherkin describe el comportamiento esperado; por sí solo no automatiza las pruebas (se necesitaría una herramienta como Behave o Cucumber).
