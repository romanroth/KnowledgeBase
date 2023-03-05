---
tags:
  - C
  - Programming
---

# C

## Code Snippets

```c
// Write to file with timestamp.
#include <stdio.h>
#include <sys/time.h>

char* g_szFilePath = "/root/canlog";
FILE* g_fileHandle = NULL;

//...

g_fileHandle = fopen(g_szFilePath, "wb");

//...

static char acLineBuffer[300];
char* pcLineBuffer = acLineBuffer;

// Timestamp
struct timeval tv;
gettimeofday(&tv, NULL);
unsigned long long millisecondsSinceEpoch =
	(unsigned long long)(tv.tv_sec) * 1000 +
	(unsigned long long)(tv.tv_usec) / 1000;
pcLineBuffer += sprintf(pcLineBuffer, "%llu ", millisecondsSinceEpoch);

// ... (put something in acLineBuffer)

fprintf(g_fileHandle, "%s\n", acLineBuffer);

```