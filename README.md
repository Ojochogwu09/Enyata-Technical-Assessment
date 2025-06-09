javascript
describe("SauceDemo E2E Test", () => {
    beforeEach(() => {
        // Visit SauceDemo before each test
        cy.visit("https://www.saucedemo.com/");
    });

    it("Login, add items to cart, checkout, and logout", () => {
        // Login with valid credentials
        cy.get("#user-name").type("standard_user");
        cy.get("#password").type("secret_sauce");
        cy.get("#login-button").click();

        // Verify successful login
        cy.url().should("include", "/inventory.html");

        // Add first item to cart
        cy.get(".inventory_item").first().find("button").click();
        cy.get(".shopping_cart_badge").should("contain", "1");

        // Go to cart page
        cy.get(".shopping_cart_link").click();
        cy.url().should("include", "/cart.html");

        // Proceed to checkout
        cy.get("#checkout").click();
        cy.url().should("include", "/checkout-step-one.html");

        // Enter checkout details
        cy.get("#first-name").type("Esther");
        cy.get("#last-name").type("Agada");
        cy.get("#postal-code").type("101010");
        cy.get("#continue").click();

        // Finish the order
        cy.url().should("include", "/checkout-step-two.html");
        cy.get("#finish").click();

        // Confirm order success message
        cy.get(".complete-header").should("contain", "Thank you for your order!");

        // Logout
        cy.get("#react-burger-menu-btn").click();
        cy.get("#logout_sidebar_link").click();

        // Verify redirection to login page
        cy.url().should("eq", "https://www.saucedemo.com/");
    });
});
