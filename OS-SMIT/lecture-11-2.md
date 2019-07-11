# Data-Centric Design

* 변수를 분산시켜서 선언하면 그만큼 페이지 숫자가 많아진다.
* 1번에 큰 페이지를 할당받고 포인터로 데이터를 관리한다.
* 페이지 내에서 변수의 위치는 Cache Align을 맞춰서 할당한다.
* 약간의 메모리 낭비는 있을 수 있지만 속도가 매우 빨라진다.

## Big Page

* 하나의 페이지는 기본 4kb이지만 4mb의 Big Page도 존재함.

```c
#define CACHE_ALIGN 8
int iMemSize;
int iBitDepth = MyAppData->Param->iBitDepth;
int iNumPartition = MyAppData->Param->iNumPart;
iMemSize = iNumPartition*sizeof(int *);
long iRemnant;
iRemnant = (long)iMemSize % CACHE_ALIGN;
if(Remnant) iMemSize += CACHE_ALIGN – iRemnant;
int iPelValue = 1 << iBitDepth;
int i;
for(i=0; i<iPelValue; i++) {
    iMemSize += iPelValue * sizeof(int);
    iRemnant = iMemSize % CACHE_ALIGN;
    if(Remnant) iMemSize += CACHE_ALIGN – iRemnant;
}

#Ifdef _WINDOWS
    Int BigPages = iMemSize / (4096 * 1024);
    iMemSize = (BigPages + 1) * 4096 * 1024;
#endif

MyAppData->Data->cpBigData = (char *)calloc(iMemSize, 1);

char *cpMemPos = MyAppData->Data->cpBigData;

MyAppData->Data->ippHistogram = (void *)cpMemPos; 
cpMemPos += iNumPartition * sizeof(int *);
iRemnant = (long)cpMemPos % CACHE_ALIGN;
If(iRemnant) cpMemPos += (CACHE_ALIGN – iRemnant);

for(i=0; i<iPelValue; i++) {
    MyAppData->Data->ippHistogram[i] = (int *)cpMemPos;
    cpMemPos += iPeoValue * sizeof(int);
    iRemnant = (long)cpMemPos % CACHE_ALIGN; 
    if(Remnant) cpMemPos += (CACHE_ALIGN – iRemnant);
}
```
