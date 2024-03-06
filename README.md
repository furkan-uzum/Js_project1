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




bash script




#!/bin/bash

# Path to the target program binary
TARGET_PROGRAM="./target_program"

# Directory containing the .cov files
COV_DIR="/home/furkan/myfs/source-codes/uzum/fuzz/hfuzz_workspace/output"

# Check if the target program binary exists
if [ ! -x "$TARGET_PROGRAM" ]; then
    echo "Error: $TARGET_PROGRAM does not exist or is not executable"
    exit 1
fi

# Check if the directory containing .cov files exists
if [ ! -d "$COV_DIR" ]; then
    echo "Error: $COV_DIR does not exist or is not a directory"
    exit 1
fi

# Iterate over all .cov files in the directory
for cov_file in "$COV_DIR"/*.cov; do
    # Check if cov_file is a regular file
    if [ -f "$cov_file" ]; then
        # Execute the target program with the current .cov file
        echo "Running fuzz with input from $cov_file"
        $TARGET_PROGRAM "$cov_file"
    fi
done