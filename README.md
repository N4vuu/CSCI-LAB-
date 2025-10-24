# CSCI-LAB-
#include <stdio.h>
#include <stdlib.h>

//--Variable definition--//

#define MIN_A 2
#define MIN_B 2
#define MIN_C 2

//--Global variables--// (defining quantities and prices)
int password = 1234;
float total_amount = 0.0;
int Qty_A = 5; 
int Qty_B = 5;
int Qty_C = 5;
float price_A = 2.00;
float price_B = 2.50;
float price_C = 3.00;

//--Function modes--// (Different types of modes both for customers and admin)

void consumerMode();
void adminMode();
void restockProducts();
void alterPrice();
void displayTotalSales();
void displayStock();

int main() {
    int choice;

    while (1) {
        printf("\n-------VENDING MACHINE -------\n");
        printf("1. Chose a Product\n");
        printf("2. Admin mode\n");
        printf("3. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                consumerMode();
                break;
            case 2:
                adminMode();
                break;
            case 3:
                printf("\nExiting!\n");
                return 0;
            default:
                printf("Invalid choice! Try again.\n");
        }
    }
}

// ------ CONSUMER MODE ------ //
void consumerMode() {
    int select;
    float inserted;
    float coin;
    float price;
    char product;

    while (1) {
        printf("---- MENU ----\n");
        printf("1. Product A  Price: %.2f AED  Qty: %d\n", price_A, Qty_A);
        printf("2. Product B  Price: %.2f AED  Qty: %d\n", price_B, Qty_B);
        printf("3. Product C  Price: %.2f AED  Qty: %d\n", price_C, Qty_C);
        printf("0. Cancel\n");
        printf("Select product: ");
        scanf("%d", &select);

        if (select == 0) return; // This code Cancels the Order if the given value is 0 //
        
        //These codes selects the product and updates the values onto the corresponding definitons//
        if (select == 1 && Qty_A > 0) { product = 'A'; price = price_A; Qty_A--; }
        else if (select == 2 && Qty_B > 0) { product = 'B'; price = price_B; Qty_B--; }
        else if (select == 3 && Qty_C > 0) { product = 'C'; price = price_C; Qty_C--; }
        else {
            printf("Invalid Number\n");
            continue;
        }

        printf("You selected Product %c, Price: %.2f AED\n", product, price);
        inserted = 0;
        
        // we ask the customer to insert any of the 3 coins and calculate the outstanding balance // 
        while (inserted < price) {
            printf("Insert coin (1, 0.5, 0.25) or -1 to cancel: ");
            scanf("%f", &coin);

            if (coin == -1) {
                printf("Order cancelled. Collect your refund: %.2f AED\n", inserted);
                // Oreder was cancelled, Stock must be updated//
                if (product=='A') Qty_A++;
                else if (product=='B') Qty_B++;
                else Qty_C++;
                return;
            }

            if (coin == 1.0 || coin == 0.5 || coin == 0.25) {
                inserted += coin;
                if (inserted < price) {
                    printf("Balance: %.2f AED\n", price-inserted);
                } 
            }
            else {
                printf("Invalid coin! Try again.\n");
            }
        }

        float change = inserted - price;
        total_amount += price;

        printf("\n/*** - Purchase Successful! - ***/\n");
        printf("Product: %c | Price: %.2f AED | Paid: %.2f AED | Change: %.2f AED\n",product, price, inserted, change);
        // This marks the end of purchse//
        
        
        // Checking if machine is low on stock //
        if ((product == 'A' && Qty_A <= MIN_A) ||
            (product == 'B' && Qty_B <= MIN_B) ||
            (product == 'C' && Qty_C <= MIN_C))
            printf("ALERT: Product %c is low in stock!\n", product);

        return; // Purchse finished, Checkout. //
    }
}

/// ------- ADMIN MODE ------- ///
void adminMode() {
    int pass;
    printf("\nEnter admin password: ");
    scanf("%d", &pass);

    if (pass != password) {
        printf("Incorrect password!\n");
        return;
    }

    int choice;
    while (1) {
        printf("\n//----- ADMIN MENU -----//\n");
        printf("1. Restock Products\n");
        printf("2. Alter Product Rates\n");
        printf("3. Display Total Sales\n");
        printf("4. Display Stock\n");
        printf("0. Exit Admin Mode\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: restockProducts(); break;
            case 2: alterPrice(); break;
            case 3: displayTotalSales(); break;
            case 4: displayStock(); break;
            case 0: return;
            default: printf("Invalid option!\n");
        }
    }
}

// ------- ADMIN FUNCTIONS ------- //

void restockProducts() {
    Qty_A =10;
    Qty_B =10;
    Qty_C =10;
    printf("Products replenished successfully!\n");
    printf("New quantities:\n");
    printf("Product A: %d\n", Qty_A);
    printf("Product B: %d\n", Qty_B);
    printf("Product C: %d\n", Qty_C);
}
// This part allows us to change the prices of products according to the admins wish //
void alterPrice() {
    printf("Enter new price for Product A: ");
    scanf("%f", &price_A);
    printf("Enter new price for Product B: ");
    scanf("%f", &price_B);
    printf("Enter new price for Product C: ");
    scanf("%f", &price_C);
    printf("Prices updated successfully!\n");
}
// This function displays the Total sales of revenue //
void displayTotalSales() {
    int reset;
    printf("Total Sales: %.2f AED\n", total_amount);
    printf("Reset total sales? (1-Yes, 0-No): ");
    scanf("%d", &reset);
    if (reset == 1) {
        total_amount = 0;
        printf("Total sales reset. Remember to collect the money.\n");
    }
}
// Displaying the Stock of products //
void displayStock() {
    printf("\n--- Product Quantities ---\n");
    printf("A: %d\n", Qty_A);
    printf("B: %d\n", Qty_B);
    printf("C: %d\n", Qty_C);
}
