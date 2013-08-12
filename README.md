GigaOm
======
public NewsLetterSubscription signin() 
  	{
			//driver.get(baseUrl + "/");
			driver.findElement(By.linkText("Have an account? Sign in")).click();
			driver.findElement(By.cssSelector("a.goauth-facebook")).click();

			String CurrentWindow=driver.getWindowHandle();
			Set<String> windowHandles = driver.getWindowHandles();

			for(String windowHandle : windowHandles)
			{
				driver.switchTo().window(windowHandle);
				if(driver.getTitle().contains("Facebook"))
				{
					try {
						common.WaitFor(By.id("email"));
					} catch (Exception e2) {
						
						e2.printStackTrace();
					}
					driver.findElement(By.id("email")).clear();
					driver.findElement(By.id("email")).sendKeys("testermactester@yahoo.com");
					driver.findElement(By.id("pass")).clear();
					driver.findElement(By.id("pass")).sendKeys("H3lloworld!");
					driver.findElement(By.name("login")).click();
					while(true)
					{
						if(driver.getWindowHandles().size()!=1)
							try {
								Thread.sleep(1000);
							} catch (InterruptedException e1) {
								e1.printStackTrace();
							}
						else
							break;
					}
				}
			}
			driver.switchTo().window(CurrentWindow);

			//driver.get(baseUrl + "/");
			for (int second = 0;; second++) {
				if (second >= 60) Assert.fail("timeout");
				try { if (common.isElementPresent(By.id("footer-newsletters"))) break; } catch (Exception e) {}
				try {
					Thread.sleep(1000);
				} catch (InterruptedException e1) {
					e1.printStackTrace();
				}
			}

			driver.findElement(By.id("footer-newsletters")).click();
			driver.findElement(By.id("all-newsletters")).click();
			driver.findElement(By.id("newsletterSave")).click();
			for (int second = 0;; second++) {
				if (second >= 60) Assert.fail("timeout");
				try { if ("Thanks for updating your subscription preferences. Your selections have been saved.".equals(driver.findElement(By.cssSelector("ul.errors > li")).getText())) break; } catch (Exception e) {}
				try {
					Thread.sleep(1000);
				} catch (InterruptedException e1) {
					e1.printStackTrace();
				}
			}

			driver.findElement(By.id("no-\"newsletters\"")).click();
			driver.findElement(By.id("newsletterSave")).click();
			for (int second = 0;; second++) {
				if (second >= 60) Assert.fail("timeout");
				try { if ("Your request to unsubscribe has been submitted successfully. Sorry to see you go.".equals(driver.findElement(By.cssSelector("ul.errors > li")).getText())) break; } catch (Exception e) {}
				try {
					Thread.sleep(1000);
				} catch (InterruptedException e1) {
					e1.printStackTrace();
				}
			}

			WebElement e=driver.findElement(By.cssSelector("#bp-adminbar-account-menu a"));
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e1) {
				e1.printStackTrace();
			}
			Actions action = new Actions(driver);
			action.moveToElement(e).build().perform();
			driver.findElement(By.id("bp-admin-logout")).click();
		
			return new NewsLetterSubscription(driver);
		}

