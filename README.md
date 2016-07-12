# TEST



public class TEST {

    public static void main(String[] args) {
        WebDriver driver = new FirefoxDriver();
        
        //open wrike.com/wa
        driver.get("http://www.wrike.com/wa/");
        driver.manage().window().maximize();
        driver.manage().timeouts().pageLoadTimeout(10, TimeUnit.SECONDS);
        
        //click Solutions
        driver.findElement(By.cssSelector("span.wg-header__desktop-primary-nav-span")).click();
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        //Product Development teams + check your URL have changed
        String prURL = driver.getCurrentUrl();
        driver.findElement(By.xpath("//*[contains(@class,'wg-header__sticky-submenu-item wg-header__sticky-submenu-item--dev')]")).click();
        if (driver.getCurrentUrl().equals(prURL))
            System.out.println("URL_not_changed");
        else
            System.out.println("URL_changed");
        
        //type invalid email in email input and click 'Get started for free'
        driver.findElement(By.xpath("//*[contains(@class,'wg-input global-form__input')]")).click();
        WebElement inputtext = driver.findElement(By.xpath("//*[contains(@class,'wg-input global-form__input')]"));
        inputtext.sendKeys("asdasdasd.ru");
        driver.findElement(By.xpath("//*[contains(@class,'wg-btn wg-btn--gray global-form__btn')]")).click();
        
        //check error message
        WebElement element = driver.findElement(By.xpath("//*[contains(@class,'wg-field-error--visible')]"));
        String errtxt = element.getText();
        System.out.println(errtxt);
        
        //refresh page
        driver.navigate().refresh();
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        //check there is no error message anymore and email input is empty
        if (driver.findElement(By.xpath("//*[contains(@class,'wg-field-error')]")).isDisplayed())
            System.out.println("error message");
        else
            System.out.println("no error message");
        WebElement inputelement = driver.findElement(By.xpath("//*[contains(@class,'wg-input global-form__input')]"));
        if (inputelement.getText().isEmpty())
            System.out.println("email input is empty");
        
        //check  that you can see text 'Robust Features for Product Development' below
        JavascriptExecutor je = (JavascriptExecutor)driver;
        je.executeScript("arguments[0].scrollIntoView(true);", driver.findElement(By.xpath("//*[contains(@class,'section-features')]")));
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        WebElement eltxt2 = driver.findElement(By.cssSelector("section.section-features > div.wg-grid > div.wg-cell > h2"));
        if (eltxt2.getText().contains("Robust Features\n"+"for Product Development"))
            System.out.println("I can see 'Robust Features for Product Development'");
        
        //click 'View Full Product'
        Set<String> oldWindowsSet = driver.getWindowHandles();
        driver.findElement(By.linkText("View Full Product")).click();
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        Set<String> newWindowsSet = driver.getWindowHandles();
        newWindowsSet.removeAll(oldWindowsSet);
        String newWindowHandle = newWindowsSet.iterator().next();
        driver.switchTo().window(newWindowHandle);
        
        //scroll the page down a bit
        je.executeScript("window.scrollBy(0,250)", "");
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        //check on the page header you can now type your email in 'Enter your business email' input
        if (driver.findElement(By.cssSelector("div.r > form.wg-header__free-trial-form.wg-form > input[name=\"email\"]")).isDisplayed())
            System.out.println("Element Visible");
        else
            System.out.println("Element not Visible");
        
        //click 'Get started for free'
        WebElement getstart = driver.findElement(By.xpath("(//button[@type='submit'])[2]"));
        Actions action = new Actions(driver);
        action.moveToElement(getstart).click().build().perform();
        
        //check error message
        WebElement errmsg2 = driver.findElement(By.xpath("//*[contains(@class,'wg-field-error--visible')]"));
        System.out.println(errmsg2.getText());
        if (errmsg2.isDisplayed())
            System.out.println("error message visible");
        
        //type valid random email in field, click "get started for free" button
        WebElement inputtext2 = driver.findElement(By.cssSelector("div.r > form.wg-header__free-trial-form.wg-form > input[name=\"email\"]"));
        inputtext2.sendKeys("evgenyswim@mail.ru");
        prURL = driver.getCurrentUrl();
        action.moveToElement(getstart).click().build().perform();
        try {
            Thread.sleep(6000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        // check at the loaded page, that you have success confirmation of registration
        if (driver.getCurrentUrl().equals(prURL))
            System.out.println("URL_not_changed");
        else
            System.out.println("URL_changed");
        if (driver.findElement(By.cssSelector(".section")).isDisplayed())
            System.out.println("you have success confirmation of registration");
    }
}
