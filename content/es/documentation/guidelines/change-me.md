---
title: "Page object models translated into Spanish"
linkTitle: "Modelos de Objetos página"
weight: 100
description: >-
 Translation into Spanish of https://www.selenium.dev/es/documentation/guidelines/page_object_models/
---

## Heading

Modelos de objetos página (Page object models POM):
* El patrón objeto página es un patrón de diseño que se ha hecho popular en la automatización de
* pruebas para mejorar el mantenimiento de las mismas y reducir la duplicación de
* código. Un objeto página es una clase orientada a objetos que sirve de interfaz
* a una página de su UAT. Las pruebas utilizan los métodos de esta clase objeto
* página cada vez que necesitan interactuar con la interfaz de usuario de esa
* página. La ventaja es que si la interfaz de usuario de la página cambia, las
* pruebas no tienen que cambiar, sólo el código dentro de ese objeto página tiene
* que cambiar. Posteriormente, todos los cambios para soportar esa nueva interfaz
* de usuario se encuentran en un solo lugar.
*El patrón de diseño de objetos página ofrece las siguientes ventajas:
*  1.-   Hay una separación limpia entre el código de prueba y el código específico de la página, como los localizadores (o su uso
*  si se utiliza un mapa de interfaz de usuario) y el diseño.
*   2.- Existe un único repositorio para los servicios u operaciones que ofrece la página en lugar de tener estos servicios
*  dispersos por las pruebas.
*
* En ambos casos, esto
* permite que las modificaciones necesarias debido a los cambios en la interfaz
* de usuario se realicen en un solo lugar. Se puede encontrar información útil
* sobre esta técnica en numerosos blogs, ya que este "patrón de diseño de
* pruebas" se está utilizando ampliamente. Animamos al lector que desee
* saber más a que busque en Internet blogs sobre este tema. Muchos han escrito
* sobre este patrón de diseño y pueden proporcionar consejos útiles que van más
* allá del alcance de esta guía de usuario. Sin embargo, para empezar,
* ilustraremos los objetos página con un ejemplo sencillo.
* Primero, consideremos un ejemplo, típico de la automatización de pruebas, que no utiliza un objeto página:

/***
 * Tests login feature
 */
public class Login {

  public void testLogin() {
    // fill login data on sign-in page
    driver.findElement(By.name("user_name")).sendKeys("testUser");
    driver.findElement(By.name("password")).sendKeys("my supersecret password");
    driver.findElement(By.name("sign-in")).click();

    // verify h1 tag is "Hello userName" after login
    driver.findElement(By.tagName("h1")).isDisplayed();
    assertThat(driver.findElement(By.tagName("h1")).getText(), is("Hello userName"));
  }
}
/**
 * Page Object encapsulates the Sign-in page.
 */
public class SignInPage {
  protected WebDriver driver;

  // <input name="user_name" type="text" value="">
  private By usernameBy = By.name("user_name");
  // <input name="password" type="password" value="">
  private By passwordBy = By.name("password");
  // <input name="sign_in" type="submit" value="SignIn">
  private By signinBy = By.name("sign_in");

  public SignInPage(WebDriver driver){
    this.driver = driver;
  }
  /**
    * Login as valid user
    *
    * @param userName
    * @param password
    * @return HomePage object
    */
  public HomePage loginValidUser(String userName, String password) {
    driver.findElement(usernameBy).sendKeys(userName);
    driver.findElement(passwordBy).sendKeys(password);
    driver.findElement(signinBy).click();
    return new HomePage(driver);
  }
}
/***
 * Tests login feature
 */
public class TestLogin {

  @Test
  public void testLogin() {
    SignInPage signInPage = new SignInPage(driver);
    HomePage homePage = signInPage.loginValidUser("userName", "password");
    assertThat(homePage.getMessageText(), is("Hello userName"));
  }

}



* Give it a good name, ending in `.md` - e.g. `getting-started.md`
* Edit the "front matter" section at the top of the page (weight controls how its ordered amongst other pages in the same directory; lowest number first).
* Add a good commit message at the bottom of the page (<80 characters; use the extended description field for more detail).
* Create a new branch so you can preview your new file and request a review via Pull Request.
