%%writefile intermediate_code.c
#include <stdio.h>
#include <ctype.h>
#include <string.h>

int precedence(char c) {
    if (c == '+' || c == '-') return 1;
    if (c == '*' || c == '/') return 2;
    if (c == '^') return 3;
    return 0;
}

void infixToPostfix(char* infix, char* postfix) {
    char stack[100];
    int top = -1, k = 0;
    for (int i = 0; infix[i]; i++) {
        if (isalnum(infix[i])) postfix[k++] = infix[i];
        else if (infix[i] == '(') stack[++top] = infix[i];
        else if (infix[i] == ')') {
            while (stack[top] != '(') postfix[k++] = stack[top--];
            top--;
        } else {
            while (top >= 0 && precedence(stack[top]) >= precedence(infix[i])) 
                postfix[k++] = stack[top--];
            stack[++top] = infix[i];
        }
    }
    while (top >= 0) postfix[k++] = stack[top--];
    postfix[k] = '\0';
}

void infixToPrefix(char* infix, char* prefix) {
    int len = strlen(infix);
    char rev[100], temp[100];
    for (int i = 0; i < len; i++) rev[i] = (infix[len-i-1] == '(') ? ')' : (infix[len-i-1] == ')') ? '(' : infix[len-i-1];
    rev[len] = '\0';
    infixToPostfix(rev, temp);
    for (int i = 0; i < strlen(temp); i++) prefix[i] = temp[strlen(temp)-i-1];
    prefix[strlen(temp)] = '\0';
}

int main() {
    char infix[100], postfix[100], prefix[100];
    printf("Enter infix: ");
    scanf("%s", infix);
    infixToPostfix(infix, postfix);
    infixToPrefix(infix, prefix);
    printf("Postfix: %s\nPrefix: %s\n", postfix, prefix);
    return 0;
}

gcc intermediate_code.c -o intermediate_code
