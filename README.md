1. Implement a circular queue,write functions: read_queue(), write_queue(), clear_queue(),When the queue is full, write queue should overwrite oldest data.

    PROGRAME

#include <stdio.h>
#include <stdlib.h>

#define MAX_SIZE 5

// Structure to represent the circular queue
struct CircularQueue {
    int *array;
    int front, rear;
    unsigned capacity;
    int size;
};

// Function to initialize a circular queue
struct CircularQueue* initializeQueue(unsigned capacity) {
    struct CircularQueue* queue = (struct CircularQueue*)malloc(sizeof(struct CircularQueue));
    queue->capacity = capacity;
    queue->array = (int*)malloc(queue->capacity * sizeof(int));
    queue->front = queue->size = 0;
    queue->rear = capacity - 1;
    return queue;
}

// Function to check if the queue is full
int isFull(struct CircularQueue* queue) {
    return (queue->size == queue->capacity);
}

// Function to check if the queue is empty
int isEmpty(struct CircularQueue* queue) {
    return (queue->size == 0);
}

// Function to write data to the queue
void write_queue(struct CircularQueue* queue, int data) {
    if (isFull(queue)) {
        // If the queue is full, overwrite the oldest data
        queue->front = (queue->front + 1) % queue->capacity;
    }

    // Move the rear pointer circularly
    queue->rear = (queue->rear + 1) % queue->capacity;

    // Add the new data to the queue
    queue->array[queue->rear] = data;
    
    // Increase the size of the queue
    queue->size++;
}

// Function to read data from the queue
int read_queue(struct CircularQueue* queue) {
    if (isEmpty(queue)) {
        printf("Queue is empty\n");
        return -1; // Return a sentinel value for an empty queue
    }

    // Get the front element of the queue
    int data = queue->array[queue->front];

    // Move the front pointer circularly
    queue->front = (queue->front + 1) % queue->capacity;

    // Decrease the size of the queue
    queue->size--;

    return data;
}

// Function to clear the queue
void clear_queue(struct CircularQueue* queue) {
    // Reset front, rear, and size to make the queue empty
    queue->front = queue->size = 0;
    queue->rear = queue->capacity - 1;
}

// Function to print the elements of the queue
void printQueue(struct CircularQueue* queue) {
    if (isEmpty(queue)) {
        printf("Queue is empty\n");
        return;
    }

    printf("Queue elements: ");
    for (int i = 0; i < queue->size; i++) {
        printf("%d ", queue->array[(queue->front + i) % queue->capacity]);
    }
    printf("\n");
}

// Driver program for testing the circular queue
int main() {
    struct CircularQueue* queue = initializeQueue(MAX_SIZE);

    write_queue(queue, 1);
    write_queue(queue, 2);
    write_queue(queue, 3);
    write_queue(queue, 4);
    write_queue(queue, 5);
    printQueue(queue);

    write_queue(queue, 6); // Overwrite the oldest data (1)
    printQueue(queue);

    int readData = read_queue(queue);
    printf("Read from queue: %d\n", readData);
    printQueue(queue);

    clear_queue(queue);
    printQueue(queue);

    free(queue->array);
    free(queue);

    return 0;
}

          OUTPUT
Queue elements: 1 2 3 4 5 
Queue elements: 2 3 4 5 6 2 
Read from queue: 2
Queue elements: 3 4 5 6 2 
Queue is empty

2. Write a function to extract the payload from the given data and return the payload data in a new array to the calling function

Given below is the data format of the input

command
Length
Data type
Data
2 bytes
2 bytes
1 byte
Length - 1 bytes
Includes Data_type paratmeter

Ex:

input_array[] = {00, 02, 00, 11, 01, 01, 02, 03, 04, 05, 06, 07, 08, 09, 10};

then output data ?​


             PROGRAME 
             
#include <stdio.h>
#include <stdlib.h>

int extract_payload(int *input_array, int input_array_length, int **output_array) {
    int command_length_offset = 3; // Offset for Command Length field
    int length = input_array[command_length_offset]; // Payload length
    *output_array = (int *)malloc(length * sizeof(int)); // Allocate memory for the output array
    int payload_offset = command_length_offset + 1; // Offset for Payload field
    for (int i = 0; i < length; i++) {
        (*output_array)[i] = input_array[payload_offset + i]; // Copy the payload data to the output array
    }
    return length; // Return the length of the payload data
}

int main() {
    int input_array[] = {0, 2, 0, 11, 1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int input_array_length = sizeof(input_array) / sizeof(input_array[0]);
    int *output_array;
    int payload_length = extract_payload(input_array, input_array_length, &output_array);
    printf("Payload data:\n");
    for (int i = 0; i < payload_length; i++) {
        printf("%02X ", output_array[i]);
    }
    printf("\n");
    free(output_array);
    return 0;
}
     
              OUTPUT
Payload data:
01 01 02 03 04 05 06 07 08 09 0A 
