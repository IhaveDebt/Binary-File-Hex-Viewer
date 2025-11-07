#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]) {
    if (argc < 2) {
        printf("Usage: %s <filename>\n", argv[0]);
        return 1;
    }

    FILE *fp = fopen(argv[1], "rb");
    if (!fp) {
        perror("Error opening file");
        return 1;
    }

    unsigned char buffer[16];
    size_t bytes;
    int offset = 0;

    while ((bytes = fread(buffer, 1, 16, fp)) > 0) {
        printf("%08x  ", offset);
        for (size_t i = 0; i < bytes; i++) printf("%02x ", buffer[i]);
        for (size_t i = bytes; i < 16; i++) printf("   ");
        printf(" |");
        for (size_t i = 0; i < bytes; i++)
            printf("%c", buffer[i] >= 32 && buffer[i] < 127 ? buffer[i] : '.');
        printf("|\n");
        offset += 16;
    }

    fclose(fp);
    return 0;
}
