# SIMPLE-VOTING-SYSTEM.
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Maximum number of candidates
#define MAX_CANDIDATES 10

// Structure for voter details
typedef struct Voter {
    int voterId;
    char name[50];
    struct Voter* next;
} Voter;

// Structure for candidate
typedef struct {
    int candidateId;
    char name[50];
    int votes;
    Voter* voters; // Linked list of voters who voted for this candidate
} Candidate;

// Global array of candidates
Candidate candidates[MAX_CANDIDATES];
int candidateCount = 0;

// Function to add a new candidate
void addCandidate(int id, const char* name) {
    if (candidateCount >= MAX_CANDIDATES) {
        printf("Maximum number of candidates reached!\n");
        return;
    }
    
    candidates[candidateCount].candidateId = id;
    strcpy(candidates[candidateCount].name, name);
    candidates[candidateCount].votes = 0;
    candidates[candidateCount].voters = NULL;
    candidateCount++;
    printf("Candidate added successfully!\n");
}

// Function to add a voter to a candidate's list
void addVoter(int candidateId, int voterId, const char* voterName) {
    // Find the candidate
    int i;
    for (i = 0; i < candidateCount; i++) {
        if (candidates[i].candidateId == candidateId) {
            // Create new voter
            Voter* newVoter = (Voter*)malloc(sizeof(Voter));
            newVoter->voterId = voterId;
            strcpy(newVoter->name, voterName);
            newVoter->next = NULL;
            
            // Add to the beginning of the linked list
            newVoter->next = candidates[i].voters;
            candidates[i].voters = newVoter;
            candidates[i].votes++;
            
            printf("Vote recorded successfully!\n");
            return;
        }
    }
    printf("Candidate not found!\n");
}

// Function to display all candidates and their votes
void displayCandidates() {
    printf("\nCandidates List:\n");
    printf("ID\tName\t\tVotes\n");
    printf("--------------------------------\n");
    
    for (int i = 0; i < candidateCount; i++) {
        printf("%d\t%s\t\t%d\n", 
               candidates[i].candidateId, 
               candidates[i].name, 
               candidates[i].votes);
    }
}

// Function to display voters for a specific candidate
void displayVoters(int candidateId) {
    int i;
    for (i = 0; i < candidateCount; i++) {
        if (candidates[i].candidateId == candidateId) {
            printf("\nVoters for %s:\n", candidates[i].name);
            printf("Voter ID\tName\n");
            printf("------------------------\n");
            
            Voter* current = candidates[i].voters;
            while (current != NULL) {
                printf("%d\t\t%s\n", current->voterId, current->name);
                current = current->next;
            }
            return;
        }
    }
    printf("Candidate not found!\n");
}

int main() {
    int choice;
    
    printf("Welcome to Election System!\n");
    
    while (1) {
        printf("\nMenu:\n");
        printf("1. Add Candidate\n");
        printf("2. Add Voter\n");
        printf("3. Display Candidates\n");
        printf("4. Display Voters for a Candidate\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        
        switch (choice) {
            case 1: {
                int id;
                char name[50];
                printf("Enter candidate ID: ");
                scanf("%d", &id);
                printf("Enter candidate name: ");
                scanf("%s", name);
                addCandidate(id, name);
                break;
            }
            case 2: {
                int candidateId, voterId;
                char voterName[50];
                printf("Enter candidate ID: ");
                scanf("%d", &candidateId);
                printf("Enter voter ID: ");
                scanf("%d", &voterId);
                printf("Enter voter name: ");
                scanf("%s", voterName);
                addVoter(candidateId, voterId, voterName);
                break;
            }
            case 3:
                displayCandidates();
                break;
            case 4: {
                int candidateId;
                printf("Enter candidate ID: ");
                scanf("%d", &candidateId);
                displayVoters(candidateId);
                break;
            }
            case 5:
                printf("Thank you for using the Election System!\n");
                return 0;
            default:
                printf("Invalid choice! Please try again.\n");
        }
    }
    
    return 0;
}
