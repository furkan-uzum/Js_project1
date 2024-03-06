# Js_project1

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include <unistd.h>

#define MAX_INPUT_SIZE 100

int main(int argc, char *argv[]) {
    if (argc != 2) {
        printf("Usage: %s <input_file>\n", argv[0]);
        return 1;
    }

    FILE *file = fopen(argv[1], "r");
    if (file == NULL) {
        fprintf(stderr, "Error opening file: %s\n", strerror(errno));
        return 1;
    }

    char input[MAX_INPUT_SIZE];
    if (fgets(input, MAX_INPUT_SIZE, file) == NULL) {
        fprintf(stderr, "Error reading from file: %s\n", strerror(errno));
        fclose(file);
        return 1;
    }
    fclose(file);

    // Remove trailing newline character
    input[strcspn(input, "\n")] = '\0';

    if (strstr(input, "SECRET") != NULL) {
        printf("Access granted!\n");
    } else {
        printf("Access denied!\n");
    }

    // Simulate some processing time
    usleep(100000); // Sleep for 10 milliseconds

    return 0;
}