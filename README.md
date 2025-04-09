# Basic-Calculator
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

#define MAX 100

// Stack structure
struct Stack {
    int top;
    float items[MAX];
};

// Stack functions
void initStack(struct Stack* stack) {
    stack->top = -1;
}

int isEmpty(struct Stack* stack) {
    return stack->top == -1;
}

void push(struct Stack* stack, float value) {
    if (stack->top == MAX - 1) {
        printf("Stack overflow\n");
        return;
    }
    stack->items[++(stack->top)] = value;
}

float pop(struct Stack* stack) {
    if (isEmpty(stack)) {
        printf("Stack underflow\n");
        return -1;
    }
    return stack->items[(stack->top)--];
}

// Function to perform operations
float performOperation(float a, float b, char operator) {
    switch (operator) {
        case '+': return a + b;
        case '-': return a - b;
        case '*': return a * b;
        case '/': return a / b;
        default: return 0;
    }
}

// Function to evaluate the expression
float evaluate(char* expression) {
    struct Stack stack;
    initStack(&stack);
    
    for (int i = 0; i < strlen(expression); i++) {
        char ch = expression[i];
        
        if (isdigit(ch)) {
            push(&stack, ch - '0'); // Push digits to stack
        } else if (ch == '+' || ch == '-' || ch == '*' || ch == '/') {
            float b = pop(&stack);
            float a = pop(&stack);
            push(&stack, performOperation(a, b, ch));
        }
    }
    return pop(&stack);  // Final result
}

int main() {
    char expression[MAX];
    printf("Enter the expression: ");
    scanf("%s", expression);

    float result = evaluate(expression);
    printf("Result: %.2f\n", result);

    return 0;
}
