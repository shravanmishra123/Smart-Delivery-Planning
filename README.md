#include <stdio.h>

#define MAX 100

typedef struct {
    int id;
    float value;
    float weight;
    float ratio;
    float fraction;
} Package;

Package p[MAX];
int n = 0, entered = 0, sorted = 0, calculated = 0;
float capacity = 0, totalValue = 0, totalWeight = 0;

void enterDetails() {
    int i;

    printf("Enter number of packages: ");
    scanf("%d", &n);

    if (n <= 0 || n > MAX) {
        printf("Invalid number of packages.\n");
        n = 0;
        entered = 0;
        return;
    }

    for (i = 0; i < n; i++) {
        p[i].id = i + 1;

        printf("Enter value and weight of package %d: ", i + 1);
        scanf("%f %f", &p[i].value, &p[i].weight);

        if (p[i].value < 0 || p[i].weight <= 0) {
            printf("Invalid value or weight. Enter details again.\n");
            i--;
            continue;
        }

        p[i].ratio = 0;
        p[i].fraction = 0;
    }

    printf("Enter vehicle capacity: ");
    scanf("%f", &capacity);

    if (capacity < 0) {
        printf("Invalid capacity.\n");
        n = 0;
        entered = 0;
        return;
    }

    entered = 1;
    sorted = 0;
    calculated = 0;
    printf("Package details entered successfully.\n");
}

void calculateRatio() {
    int i;

    if (!entered) {
        printf("Please enter package details first.\n");
        return;
    }

    for (i = 0; i < n; i++) {
        p[i].ratio = p[i].value / p[i].weight;
    }

    printf("\nValue/Weight ratios calculated successfully.\n");
}

void displayDetails() {
    int i;

    if (!entered) {
        printf("Please enter package details first.\n");
        return;
    }

    printf("\nPackage Details\n");
    printf("ID\tValue\tWeight\tRatio\n");

    for (i = 0; i < n; i++) {
        printf("%d\t%.2f\t%.2f\t%.2f\n",
               p[i].id, p[i].value,
               p[i].weight, p[i].ratio);
    }
}

void sortPackages() {
    int i, j;
    Package temp;

    if (!entered) {
        printf("Please enter package details first.\n");
        return;
    }

    for (i = 0; i < n; i++) {
        p[i].ratio = p[i].value / p[i].weight;
    }

    for (i = 0; i < n - 1; i++) {
        for (j = 0; j < n - i - 1; j++) {
            if (p[j].ratio < p[j + 1].ratio) {
                temp = p[j];
                p[j] = p[j + 1];
                p[j + 1] = temp;
            }
        }
    }

    sorted = 1;
    calculated = 0;

    printf("\nPackages sorted by decreasing Value/Weight ratio.\n");
    displayDetails();
}

void findMaximumValue() {
    int i;
    float remaining, fraction;

    if (!entered) {
        printf("Please enter package details first.\n");
        return;
    }

    if (!sorted) {
        sortPackages();
    }

    remaining = capacity;
    totalValue = 0;
    totalWeight = 0;

    for (i = 0; i < n; i++) {
        p[i].fraction = 0;

        if (remaining <= 0)
            break;

        if (p[i].weight <= remaining) {
            p[i].fraction = 1;
            totalValue += p[i].value;
            totalWeight += p[i].weight;
            remaining -= p[i].weight;
        } else {
            fraction = remaining / p[i].weight;
            p[i].fraction = fraction;
            totalValue += p[i].value * fraction;
            totalWeight += remaining;
            remaining = 0;
        }
    }

    calculated = 1;

    printf("\nMaximum Value = %.2f\n", totalValue);
    printf("Total Weight Used = %.2f\n", totalWeight);
}

void displaySelectedPackages() {
    int i;

    if (!calculated) {
        printf("Please calculate the maximum value first.\n");
        return;
    }

    printf("\nSelected Packages\n");
    printf("ID\tFraction\tWeight Taken\tValue Obtained\n");

    for (i = 0; i < n; i++) {
        if (p[i].fraction > 0) {
            printf("%d\t%.2f\t\t%.2f\t\t%.2f\n",
                   p[i].id,
                   p[i].fraction,
                   p[i].weight * p[i].fraction,
                   p[i].value * p[i].fraction);
        }
    }

    printf("\nTotal Weight Used = %.2f\n", totalWeight);
    printf("Maximum Value Obtained = %.2f\n", totalValue);
}

int main() {
    int choice;

    do {
        printf("\n===== SMART DELIVERY PLANNING =====\n");
        printf("1. Enter Package Details\n");
        printf("2. Display Package Details\n");
        printf("3. Calculate Value/Weight Ratio\n");
        printf("4. Sort Packages by Ratio\n");
        printf("5. Find Maximum Value\n");
        printf("6. Display Selected Packages\n");
        printf("7. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                enterDetails();
                break;

            case 2:
                displayDetails();
                break;

            case 3:
                calculateRatio();
                break;

            case 4:
                sortPackages();
                break;

            case 5:
                findMaximumValue();
                break;

            case 6:
                displaySelectedPackages();
                break;

            case 7:
                printf("Exiting program.\n");
                break;

            default:
                printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 7);

    return 0;
}
