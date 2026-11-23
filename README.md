# mini-project


#include <stdio.h>

#define DAYS 7
#define ITEMS 3

int main() {
    int sold[DAYS][ITEMS] = {0};
    int price[ITEMS] = {10, 15, 12};
    int choice, day, i, j;
    FILE *fp;

    // Load data at start if file exists
    fp = fopen("sales.txt", "r");
    if (fp != NULL) {
        for (i = 0; i < DAYS; i++) {
            for (j = 0; j < ITEMS; j++) {
                fscanf(fp, "%d", &sold[i][j]);
            }
        }
        fclose(fp);
    }

    do {
        printf("\n--- Chai Sales Log ---\n");
        printf("1. Add day's sales\n");
        printf("2. Weekly report\n");
        printf("3. Save\n");
        printf("4. Exit\n");
        printf("5. Bar chart\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter day (1 to %d): ", DAYS);
                scanf("%d", &day);

                if (day < 1 || day > DAYS) {
                    printf("Invalid day! Please enter between 1 and %d.\n", DAYS);
                } else {
                    printf("Enter Tea, Coffee and Samosa sold: ");
                    for (j = 0; j < ITEMS; j++) {
                        scanf("%d", &sold[day - 1][j]);
                    }
                    printf("Sales added for Day %d.\n", day);
                }
                break;

            case 2: {
                int weekly_rev = 0;
                int best_day = 1;
                int max_rev = -1;

                printf("\nDay\tTea\tCoffee\tSamosa\tRevenue\n");
                for (i = 0; i < DAYS; i++) {
                    int day_rev = 0;
                    for (j = 0; j < ITEMS; j++) {
                        day_rev += sold[i][j] * price[j];
                    }

                    printf("%d\t%d\t%d\t%d\tRs.%d\n", i + 1, sold[i][0], sold[i][1], sold[i][2], day_rev);

                    weekly_rev += day_rev;
                    if (day_rev > max_rev) {
                        max_rev = day_rev;
                        best_day = i + 1;
                    }
                }

                printf("\nWeekly Revenue: Rs.%d\n", weekly_rev);
                printf("Best Day: Day %d (Rs.%d)\n", best_day, max_rev);
                break;
            }

            case 3:
                fp = fopen("sales.txt", "w");
                if (fp == NULL) {
                    printf("Error opening file!\n");
                } else {
                    for (i = 0; i < DAYS; i++) {
                        for (j = 0; j < ITEMS; j++) {
                            fprintf(fp, "%d ", sold[i][j]);
                        }
                        fprintf(fp, "\n");
                    }
                    fclose(fp);
                    printf("Saved to sales.txt\n");
                }
                break;

            case 4:
                printf("Exiting...\n");
                break;

            case 5: {
                printf("\n--- Revenue Bar Chart (1 * = Rs.20) ---\n");
                for (i = 0; i < DAYS; i++) {
                    int day_rev = 0;
                    for (j = 0; j < ITEMS; j++) {
                        day_rev += sold[i][j] * price[j];
                    }

                    int stars = day_rev / 20;
                    printf("Day %d: ", i + 1);
                    for (j = 0; j < stars; j++) {
                        printf("*");
                    }
                    printf(" (Rs.%d)\n", day_rev);
                }
                break;
            }

            default:
                printf("Wrong choice! Try again.\n");
        }

    } while (choice != 4);

    return 0;
}
