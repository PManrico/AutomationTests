# AutomationTests

Below are some Automated Test Case samples that I wrote while I was working on previous projects.

describe('Login tests', () => {

  it('Login cu user sau parola gresita - arata eroare', () => {
    cy.visit('https://www.saucedemo.com/');
    cy.get('#user-name').type('usergresit');
    cy.get('#password').type('parolagresita');
    cy.get('#login-button').click();
    cy.get('[data-test="error"]').should('contain', 'Epic sadface');
  });

  it('Login cu userul standard - succes', () => {
    cy.visit('https://www.saucedemo.com/');
    cy.get('#user-name').type('standard_user');
    cy.get('#password').type('secret_sauce');
    cy.get('#login-button').click();
    cy.url().should('include', 'inventory.html');
  });

});


-------------------------------------------


describe('Logout test', () => {

  beforeEach(() => {
    cy.visit('https://www.saucedemo.com/');
    cy.get('#user-name').type('standard_user');
    cy.get('#password').type('secret_sauce');
    cy.get('#login-button').click();
  });

  it('Logout dupa login', () => {
    cy.get('#react-burger-menu-btn').click();
    cy.get('#logout_sidebar_link').click();
    cy.url().should('eq', 'https://www.saucedemo.com/');
  });

});


-----------------------------------------


describe('Meniu lateral', () => {

  beforeEach(() => {
    cy.visit('https://www.saucedemo.com/');
    cy.get('#user-name').type('standard_user');
    cy.get('#password').type('secret_sauce');
    cy.get('#login-button').click();
  });

  it('Deschidere si inchidere meniu lateral', () => {
    cy.get('#react-burger-menu-btn').click();
    cy.get('.bm-menu-wrap').should('be.visible');
    cy.get('#react-burger-cross-btn').click();
    cy.get('.bm-menu-wrap').should('not.be.visible');
  });

});


----------------------------------------


describe('Cart tests', () => {

  beforeEach(() => {
    cy.visit('https://www.saucedemo.com/');
    cy.get('#user-name').type('standard_user');
    cy.get('#password').type('secret_sauce');
    cy.get('#login-button').click();
  });

  it('Adaugare produs in cart', () => {
    cy.get('#add-to-cart-sauce-labs-backpack').click();
    cy.get('.shopping_cart_badge').should('have.text', '1');
  });

  it('Stergere produs din cart', () => {
    cy.get('#add-to-cart-sauce-labs-backpack').click();
    cy.get('.shopping_cart_link').click();
    cy.get('#remove-sauce-labs-backpack').click();
    cy.get('.cart_item').should('not.exist');
  });

});



-------------------------------------------------------


describe('Checkout test', () => {

  beforeEach(() => {
    cy.visit('https://www.saucedemo.com/');
    cy.get('#user-name').type('standard_user');
    cy.get('#password').type('secret_sauce');
    cy.get('#login-button').click();
  });

  it('Plasare comanda cu succes', () => {
    cy.get('#add-to-cart-sauce-labs-backpack').click();
    cy.get('.shopping_cart_link').click();
    cy.get('#checkout').click();

    cy.get('#first-name').type('Ion');
    cy.get('#last-name').type('Popescu');
    cy.get('#postal-code').type('12345');
    cy.get('#continue').click();
    cy.get('#finish').click();

    cy.get('.complete-header').should('have.text', 'Thank you for your order!');
  });


-----------------------------------------------


describe('Product page tests', () => {

  beforeEach(() => {
    cy.visit('https://www.saucedemo.com/');
    cy.get('#user-name').type('standard_user');
    cy.get('#password').type('secret_sauce');
    cy.get('#login-button').click();
  });

  it('Accesare pagina detalii produs', () => {
    cy.get('.inventory_item_name').first().click();
    cy.url().should('include', 'inventory-item.html');
  });

  it('Buton Back to products revine la pagina principala', () => {
    cy.get('.inventory_item_name').first().click();
    cy.get('#back-to-products').click();
    cy.url().should('include', 'inventory.html');
  });

});
