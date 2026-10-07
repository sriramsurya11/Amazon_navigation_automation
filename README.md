# Amazon_navigation_automation

#Code
```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time

driver = webdriver.Chrome()
driver.maximize_window()

wait = WebDriverWait(driver, 20)

driver.get("https://www.amazon.in/")

login = wait.until(
    EC.element_to_be_clickable(
        (By.ID, "nav-link-accountList")
    )
)

login.click()

email = wait.until(
    EC.visibility_of_element_located(
        (By.ID, "ap_email_login")
    )
)

email.send_keys("")

continue_button = wait.until(
    EC.element_to_be_clickable(
        (By.CSS_SELECTOR, "input[type='submit']")
    )
)

continue_button.click()

password = wait.until(
    EC.visibility_of_element_located(
        (By.ID, "ap_password")
    )
)

password.send_keys("")

print("Password entered")

sign_in = wait.until(
    EC.element_to_be_clickable(
        (By.ID, "signInSubmit")
    )
)

sign_in.click()

print("Sign-in clicked")

search = wait.until(
    EC.element_to_be_clickable(
        (By.ID, "twotabsearchtextbox")
    )
)

print("Login successful")

search.clear()

search.send_keys("OnePlus Nord Buds")

search.send_keys(Keys.ENTER)

print("Product searched")

wait.until(
    EC.url_contains("s?k=")
)

print("Search page loaded")

print("Current URL:", driver.current_url)


products = wait.until(
    EC.presence_of_all_elements_located(
        (By.CSS_SELECTOR, "[data-asin]")
    )
)

print(
    "Product containers found:",
    len(products)
)


product_url = None

for product in products:

    asin = product.get_attribute("data-asin")

    if asin and len(asin) == 10:

        links = product.find_elements(
            By.CSS_SELECTOR,
            "a"
        )

        for link in links:

            product_text = link.text.strip()

            if (
                product_text
                and "OnePlus Nord Buds" in product_text
            ):

                product_url = link.get_attribute(
                    "href"
                )

                print("Product found:")
                print(product_text)

                print("ASIN:", asin)

                print(
                    "Product URL:",
                    product_url
                )

                break

    if product_url:
        break

if product_url is None:

    print("Product not found")

    input(
        "Press Enter to close browser..."
    )

    driver.quit()
    exit()


driver.get(product_url)

print("Opening product page...")


product_title = wait.until(
    EC.visibility_of_element_located(
        (By.ID, "productTitle")
    )
)

print("Product page opened")

print(
    "Product:",
    product_title.text.strip()
)

add_to_cart = wait.until(
    EC.element_to_be_clickable(
        (By.ID, "add-to-cart-button")
    )
)

add_to_cart.click()

print("Product added to cart")

time.sleep(4)


driver.get(
    "https://www.amazon.in/gp/cart/view.html"
)

print("Opening cart...")

time.sleep(4)

print(
    "Cart URL:",
    driver.current_url
)

print(
    "Cart title:",
    driver.title
)


proceed = wait.until(
    EC.element_to_be_clickable(
        (By.NAME, "proceedToRetailCheckout")
    )
)

proceed.click()

print("Proceeding to checkout...")

time.sleep(5)

print(
    "Checkout URL:",
    driver.current_url
)

print(
    "Checkout title:",
    driver.title
)
input(
    "Press Enter to close the browser..."
)

driver.quit()
```
#output
<img width="1600" height="857" alt="WhatsApp Image 2026-10-06 at 2 50 19 PM" src="https://github.com/user-attachments/assets/69ab0a29-37aa-4536-9fcb-deaf942613ae" />
