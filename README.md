1. Implement a circular queue,write functions: read_queue(), write_queue(), clear_queue(),When the queue is full, write queue should overwrite oldest data.

    PROGRAME
#include <stdio.h>
#include <stdlib.h>

#define MAX_QUEUE_SIZE 5

typedef struct {
    int* array;
    int front;
    int rear;
    int size;
} CircularQueue;

// Function to initialize the circular queue
void initializeQueue(CircularQueue* queue, int size) {
    queue->array = (int*)malloc(size * sizeof(int));
    queue->front = -1;
    queue->rear = -1;
    queue->size = size;
}

// Function to check if the queue is empty
int isEmpty(CircularQueue* queue) {
    return (queue->front == -1);
}

// Function to check if the queue is full
int isFull(CircularQueue* queue) {
    return ((queue->rear + 1) % queue->size == queue->front);
}

// Function to write data to the circular queue
void writeQueue(CircularQueue* queue, int data) {
    if (isFull(queue)) {
        // If the queue is full, overwrite the oldest data
        queue->front = (queue->front + 1) % queue->size;
    }

    // Move the rear pointer and write data
    queue->rear = (queue->rear + 1) % queue->size;
    queue->array[queue->rear] = data;

    // If the queue was empty, set the front pointer
    if (isEmpty(queue)) {
        queue->front = queue->rear;
    }
}

// Function to read data from the circular queue
int readQueue(CircularQueue* queue) {
    int data = -1;

    if (!isEmpty(queue)) {
        data = queue->array[queue->front];

        // If there was only one element in the queue, reset front and rear pointers
        if (queue->front == queue->rear) {
            queue->front = -1;
            queue->rear = -1;
        } else {
            // Move the front pointer
            queue->front = (queue->front + 1) % queue->size;
        }
    }

    return data;
}

// Function to clear the circular queue
void clearQueue(CircularQueue* queue) {
    queue->front = -1;
    queue->rear = -1;
}

// Function to display the circular queue
void displayQueue(CircularQueue* queue) {
    int i;
    if (isEmpty(queue)) {
        printf("Queue is empty\n");
    } else {
        printf("Queue elements: ");
        for (i = queue->front; i != queue->rear; i = (i + 1) % queue->size) {
            printf("%d ", queue->array[i]);
        }
        printf("%d\n", queue->array[i]);
    }
}

// Function to free memory used by the circular queue
void freeQueue(CircularQueue* queue) {
    free(queue->array);
}

int main() {
    CircularQueue myQueue;
    initializeQueue(&myQueue, MAX_QUEUE_SIZE);

    // Writing data to the queue
    writeQueue(&myQueue, 1);
    writeQueue(&myQueue, 2);
    writeQueue(&myQueue, 3);
    displayQueue(&myQueue);

    // Reading data from the queue
    int readData = readQueue(&myQueue);
    printf("Read data: %d\n", readData);
    displayQueue(&myQueue);

    // Writing more data to the queue
    writeQueue(&myQueue, 4);
    writeQueue(&myQueue, 5);
    writeQueue(&myQueue, 6);
    displayQueue(&myQueue);

    // Clearing the queue
    clearQueue(&myQueue);
    displayQueue(&myQueue);

    // Freeing memory
    freeQueue(&myQueue);

    return 0;
}
          OUTPUT
Queue elements: 1 2 3
Read data: 1
Queue elements: 2 3
Queue elements: 2 3 4 5 6
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
