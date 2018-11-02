### Variant 25
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>
#include <sys/wait.h>
#include <stdlib.h>
#include <string.h>

int nbytes=0;
char string[500];
void work();

int main(){
	work();
	
	return 0;

}
void work(){
	int A[2]; // pipe A
	pipe(A);
	
	pid_t pid1 = fork(); // cat
	if (!pid1) {
		dup2(A[1], 1);
		close(A[0]);
		close(A[1]);
		execlp("cat","cat", "log.txt", NULL);
		
	}

	int B[2];
	pipe(B);
	
	pid_t pid2 = fork();
	if(!pid2){
		dup2(A[0], 0);
		close(A[0]);
		close(A[1]);

		dup2(B[1], 1);
		close(B[0]);
		close(B[1]);

		execlp("grep","grep","-e","18/Oct/2006", NULL);

	}
	close(A[0]);
	close(A[1]);

	int C[2];
	pipe(C);
	pid_t pid3 = fork();
	if(!pid3){
		dup2(B[0], 0);
		close(B[0]);
		close(B[1]);

		dup2(C[1], 1);
		close(C[0]);
		close(C[1]);//file descriptor

		execlp("cut","cut","-d ","-f11", NULL);

	}
	close(B[0]);
	close(B[1]);

	int D[2];
	pipe(D);
	pid_t pid4 = fork();
	if(!pid4){
		dup2(C[0], 0);
		close(C[0]);
		close(C[1]);

		dup2(D[1], 1);
		close(D[0]);
		close(D[1]);

		execlp("tr","tr","-d","[", NULL);

	}

	close(C[0]);
	close(C[1]);


	int E[2];
	pipe(E);
	pid_t pid5 = fork();
	if(!pid5){
		dup2(D[0], 0);
		close(D[0]);
		close(D[1]);

		dup2(E[1], 1);
		close(E[0]);
		close(E[1]);

		execlp("sort","sort","-k1", NULL);

	}
	
	close(D[0]);
	close(D[1]);

	int F[2];
	pipe(F);
	pid_t pid6 = fork();
	if(!pid6){
		dup2(E[0], 0);
		close(E[0]);
		close(E[1]);

		dup2(F[1], 1);
		close(F[0]);
		close(F[1]);

		execlp("awk","awk", "{if(address != $1){print address,count;address = $1;count =1;}else{count ++;}}",NULL);

	}
	
	close(E[0]);
	close(E[1]);

	int H[2];
	pipe(H);
	pid_t pid7 = fork();
	if(!pid7){
		dup2(F[0], 0);
		close(F[0]);
		close(F[1]);

		dup2(H[1], 1);
		close(H[0]);
		close(H[1]);

		execlp("sort","sort","-k2","-n","-r", NULL);

	}
	
	close(F[0]);
	close(F[1]);

	int I[2];
	pipe(I);
	pid_t pid8 = fork();
	if(!pid8){
		dup2(H[0], 0);
		close(H[0]);
		close(H[1]);

		dup2(I[1], 1);
		close(I[0]);
		close(I[1]);

		execlp("head","head", NULL);

	}
	
	close(H[0]);
	close(H[1]);

	int U[2];
	pipe(U);
	pid_t pid9 = fork();
	if(!pid9){
		dup2(I[0], 0);
		close(I[0]);
		close(I[1]);

		dup2(U[1], 1);
		close(U[0]);
		close(U[1]);

		execlp("head","head","-4", NULL);

	}
	
	close(I[0]);
	close(I[1]);

	int Y[2];
	pipe(Y);
	pid_t pid10 = fork();
	if(!pid10){
		dup2(U[0], 0);
		close(U[0]);
		close(U[1]);

		dup2(Y[1], 1);
		close(Y[0]);
		close(Y[1]);

		execlp("awk","awk","{print $1,\"-\",$2,\"-\",$2/333*100,\"%\";}", NULL);

	}
	
	close(U[0]);
	close(U[1]);
	close(Y[1]);

	while ((nbytes = read(Y[0], string, 500)) > 0) 
            printf("%s\n", string); 
	close(Y[0]);
}


