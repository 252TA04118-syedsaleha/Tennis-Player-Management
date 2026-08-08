#include <stdio.h>
#include <string.h>

#define MAX_PLAYERS 100
#define MAX_MATCHES 100

struct Player {
    int id;
    char name[50];
    char country[30];
    int age;
    int ranking;
    int matchesPlayed;
    int wins;
    int losses;
};

struct Match {
    int matchId;
    char player1[50];
    char player2[50];
    int player1Sets;
    int player2Sets;
    char winner[50];
    char tournament[50];
};

struct Player players[MAX_PLAYERS];
struct Match matches[MAX_MATCHES];

int playerCount = 0;
int matchCount = 0;


/* Add player */
void addPlayer() {

    if (playerCount >= MAX_PLAYERS) {
        printf("\nPlayer limit reached!\n");
        return;
    }

    printf("\n========== ADD TENNIS PLAYER ==========\n");

    printf("Enter Player ID: ");
    scanf("%d", &players[playerCount].id);

    printf("Enter Player Name: ");
    scanf(" %[^\n]", players[playerCount].name);

    printf("Enter Country: ");
    scanf(" %[^\n]", players[playerCount].country);

    printf("Enter Age: ");
    scanf("%d", &players[playerCount].age);

    printf("Enter World Ranking: ");
    scanf("%d", &players[playerCount].ranking);

    players[playerCount].matchesPlayed = 0;
    players[playerCount].wins = 0;
    players[playerCount].losses = 0;

    playerCount++;

    printf("\nPlayer added successfully!\n");
}


/* Display one player */
void displayPlayer(struct Player p) {

    printf("\n----------------------------------------\n");
    printf("Player ID       : %d\n", p.id);
    printf("Name            : %s\n", p.name);
    printf("Country         : %s\n", p.country);
    printf("Age             : %d\n", p.age);
    printf("Ranking         : %d\n", p.ranking);
    printf("Matches Played  : %d\n", p.matchesPlayed);
    printf("Wins            : %d\n", p.wins);
    printf("Losses          : %d\n", p.losses);

    if (p.matchesPlayed > 0) {
        printf("Win Percentage  : %.2f%%\n",
               ((float)p.wins / p.matchesPlayed) * 100);
    } else {
        printf("Win Percentage  : 0.00%%\n");
    }
}


/* Display all players */
void displayPlayers() {

    int i;

    if (playerCount == 0) {
        printf("\nNo player records available.\n");
        return;
    }

    printf("\n========== TENNIS PLAYERS ==========\n");

    for (i = 0; i < playerCount; i++) {
        displayPlayer(players[i]);
    }
}


/* Search player */
void searchPlayer() {

    int id;
    int i;
    int found = 0;

    printf("\nEnter Player ID: ");
    scanf("%d", &id);

    for (i = 0; i < playerCount; i++) {

        if (players[i].id == id) {

            printf("\n========== PLAYER FOUND ==========\n");
            displayPlayer(players[i]);

            found = 1;
            break;
        }
    }

    if (!found) {
        printf("\nPlayer not found!\n");
    }
}


/* Search player by country */
void searchByCountry() {

    char country[30];
    int i;
    int found = 0;

    printf("\nEnter Country: ");
    scanf(" %[^\n]", country);

    printf("\n========== PLAYERS FROM %s ==========\n",
           country);

    for (i = 0; i < playerCount; i++) {

        if (strcmp(players[i].country, country) == 0) {

            displayPlayer(players[i]);
            found = 1;
        }
    }

    if (!found) {
        printf("\nNo players found from this country.\n");
    }
}


/* Add match */
void addMatch() {

    if (matchCount >= MAX_MATCHES) {
        printf("\nMatch storage is full!\n");
        return;
    }

    printf("\n========== ADD MATCH ==========\n");

    printf("Enter Match ID: ");
    scanf("%d", &matches[matchCount].matchId);

    printf("Enter Player 1 Name: ");
    scanf(" %[^\n]", matches[matchCount].player1);

    printf("Enter Player 2 Name: ");
    scanf(" %[^\n]", matches[matchCount].player2);

    printf("Enter Player 1 Sets Won: ");
    scanf("%d", &matches[matchCount].player1Sets);

    printf("Enter Player 2 Sets Won: ");
    scanf("%d", &matches[matchCount].player2Sets);

    printf("Enter Tournament Name: ");
    scanf(" %[^\n]", matches[matchCount].tournament);

    if (matches[matchCount].player1Sets >
        matches[matchCount].player2Sets) {

        strcpy(matches[matchCount].winner,
               matches[matchCount].player1);

    } else {

        strcpy(matches[matchCount].winner,
               matches[matchCount].player2);
    }

    matchCount++;

    printf("\nMatch added successfully!\n");
}


/* Display matches */
void displayMatches() {

    int i;

    if (matchCount == 0) {
        printf("\nNo match records available.\n");
        return;
    }

    printf("\n========== MATCH RECORDS ==========\n");

    for (i = 0; i < matchCount; i++) {

        printf("\n----------------------------------------\n");
        printf("Match ID      : %d\n", matches[i].matchId);
        printf("Tournament    : %s\n", matches[i].tournament);
        printf("Player 1      : %s\n", matches[i].player1);
        printf("Player 2      : %s\n", matches[i].player2);

        printf("Score         : %d - %d\n",
               matches[i].player1Sets,
               matches[i].player2Sets);

        printf("Winner        : %s\n",
               matches[i].winner);
    }
}


/* Find highest ranked player */
void highestRankedPlayer() {

    int i;
    int best;

    if (playerCount == 0) {
        printf("\nNo player records available.\n");
        return;
    }

    best = 0;

    for (i = 1; i < playerCount; i++) {

        if (players[i].ranking <
            players[best].ranking) {

            best = i;
        }
    }

    printf("\n========== HIGHEST RANKED PLAYER ==========\n");
    displayPlayer(players[best]);
}


/* Find player with most wins */
void mostWins() {

    int i;
    int best;

    if (playerCount == 0) {
        printf("\nNo player records available.\n");
        return;
    }

    best = 0;

    for (i = 1; i < playerCount; i++) {

        if (players[i].wins >
            players[best].wins) {

            best = i;
        }
    }

    printf("\n========== PLAYER WITH MOST WINS ==========\n");
    displayPlayer(players[best]);
}


/* Update player statistics */
void updateStatistics() {

    int id;
    int i;
    int result;

    printf("\nEnter Player ID: ");
    scanf("%d", &id);

    for (i = 0; i < playerCount; i++) {

        if (players[i].id == id) {

            printf("\n1. Player Won\n");
            printf("2. Player Lost\n");

            printf("Enter result: ");
            scanf("%d", &result);

            players[i].matchesPlayed++;

            if (result == 1) {
                players[i].wins++;
                printf("\nWin recorded successfully!\n");
            }
            else if (result == 2) {
                players[i].losses++;
                printf("\nLoss recorded successfully!\n");
            }
            else {
                printf("\nInvalid result!\n");
                players[i].matchesPlayed--;
            }

            return;
        }
    }

    printf("\nPlayer not found!\n");
}


/* Display ranking list */
void rankingList() {

    int i;

    if (playerCount == 0) {
        printf("\nNo player records available.\n");
        return;
    }

    printf("\n========== TENNIS RANKING ==========\n");
    printf("\nRank\tPlayer\t\tCountry\n");
    printf("--------------------------------------\n");

    for (i = 0; i < playerCount; i++) {

        printf("%d\t%-15s\t%s\n",
               players[i].ranking,
               players[i].name,
               players[i].country);
    }
}


/* Main function */
int main() {

    int choice;

    printf("================================================\n");
    printf("           TENNIS MANAGEMENT SYSTEM\n");
    printf("================================================\n");

    do {

        printf("\n=============== MAIN MENU ===============\n");
        printf("1.  Add Tennis Player\n");
        printf("2.  Display All Players\n");
        printf("3.  Search Player\n");
        printf("4.  Search Player by Country\n");
        printf("5.  Add Match\n");
        printf("6.  Display Matches\n");
        printf("7.  Find Highest Ranked Player\n");
        printf("8.  Find Player with Most Wins\n");
        printf("9.  Update Player Statistics\n");
        printf("10. Display Ranking List\n");
        printf("11. Exit\n");
        printf("==========================================\n");

        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {

            case 1:
                addPlayer();
                break;

            case 2:
                displayPlayers();
                break;

            case 3:
                searchPlayer();
                break;

            case 4:
                searchByCountry();
                break;

            case 5:
                addMatch();
                break;

            case 6:
                displayMatches();
                break;

            case 7:
                highestRankedPlayer();
                break;

            case 8:
                mostWins();
                break;

            case 9:
                updateStatistics();
                break;

            case 10:
                rankingList();
                break;

            case 11:
                printf("\nThank you for using Tennis Management System!\n");
                break;

            default:
                printf("\nInvalid choice! Please try again.\n");
        }

    } while (choice != 11);

    return 0;
}
