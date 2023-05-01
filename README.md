# Customer-Billing-System-using-file-handling
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_ITEMS 10
#define MAX_ITEM_NAME_LEN 50
// Structure to hold an item
struct Item {
 char name[MAX_ITEM_NAME_LEN];
 float price;
};
// Function to display the available items
void display_items() {
 FILE *fp = fopen("items.txt", "r");
 if (fp == NULL) {
 printf("Error: Failed to open file 'items.txt'.\n");
 return;
 }
 printf("Available items:\n");
 printf("-----------------\n");
 char buffer[MAX_ITEM_NAME_LEN];
 float price;
 while (fscanf(fp, "%s %f", buffer, &price) != EOF) {
 printf("%s - %.2f\n", buffer, price);
 }
 fclose(fp);
}
// Function to calculate the total price of the bill
float calculate_total(struct Item items[], int num_items) {
 float total = 0;
 for (int i = 0; i < num_items; i++) {
 total += items[i].price;
 }
 return total;
}
// Function to delete an item from the bill
void delete_item(struct Item items[], int *num_items) {
 int item_num;
 printf("Enter the item number to delete: ");
 scanf("%d", &item_num);
 item_num--;
 if (item_num < 0 || item_num >= *num_items) {
 printf("Error: Invalid item number.\n");
 return;
 }
 for (int i = item_num; i < *num_items - 1; i++) {
 items[i] = items[i+1];
 }
 (*num_items)--;
 printf("Item deleted.\n");
}
// Function to apply discount if applicable
float apply_discount(float total) {
 if (total > 100) {
 return total * 0.9;
 } else {
 return total;
 }
}
int main() {
 // Load items from file
 FILE *fp = fopen("items.txt", "r");
 if (fp == NULL) {
 printf("Error: Failed to open file 'items.txt'.\n");
 return 1;
 }
 struct Item items[MAX_ITEMS];
 int num_items = 0;
 char buffer[MAX_ITEM_NAME_LEN];
 float price;
 while (fscanf(fp, "%s %f", buffer, &price) != EOF) {
 if (num_items >= MAX_ITEMS) {
 printf("Error: Too many items.\n");
 fclose(fp);
 return 1;
 }
 strcpy(items[num_items].name, buffer);
 items[num_items].price = price;
 num_items++;
 }
 fclose(fp);
 // Start billing process
 printf("Welcome to the billing system!\n");
 printf("=============================\n");
 int done = 0;
 int choice;
 struct Item new_item;
 float total = 0;
 while (!done) {
// Display menu
printf("\nMenu:\n");
printf("1. Display available items\n");
printf("2. Add an item\n");
printf("3. Delete an item\n");
printf("4. Apply discount\n");
printf("5. Payment mode\n");
printf("6. Exit\n");
printf("Enter your choice: ");
scanf("%d", &choice);
 switch (choice) {
 case 1:
 // Display available items
 display_items();
 break;
 case 2:
 // Add an item
 printf("Enter item name: ");
 scanf("%s", new_item.name);
 printf("Enter item price: ");
 scanf("%f", &new_item.price);
 if (num_items >= MAX_ITEMS) {
 printf("Error: Too many items.\n");
 } else {
 items[num_items] = new_item;
 num_items++;
 total += new_item.price;
 }
 break;
 case 3:
 // Delete an item
 delete_item(items, &num_items);
 break;
 case 4:
 // Apply discount
 total = apply_discount(total);
 printf("Discount applied.\n");
 break;
 case 5:
 // Payment mode
 printf("Total amount due: %.2f\n", total);
 printf("Enter payment mode (1 = cash, 2 = card): ");
 int payment_mode;
 scanf("%d", &payment_mode);
 if (payment_mode == 1) {
 printf("Payment received. Thank you!\n");
 done = 1;
 } else if (payment_mode == 2) {
 printf("Please swipe your card.\n");
 printf("Payment received. Thank you!\n");
 done = 1;
 } else {
 printf("Invalid payment mode.\n");
 }
 break;
 case 6:
 // Exit
 done = 1;
 break;
 default:
 printf("Invalid choice.\n");
 break;
 }
}
printf("Goodbye!\n");
return 0;
}
