# Ledi  
Decoder 
	int nums[11] = [96, // => 0 
					97, // 96 + 1 => 1 
					98, // => 2 
					99, //... 
					100, 
					101, 
					102, 
					103, 
					104, 
					105, //... => 9 
					224]; 
					
void decoder (float result) {
	char r[3];
	r[0] = ftostr(result, 0);
	r[1] = ftostr(result, 1); 
	r[2] = ftostr(result, 2); 
	
	rnd(result, r); //rounding + dot 
	int x[7]; 
	ctob(r[0], &x);
	send(&x, L1);
	ctob(r[1], &x);
	send(&x, L2);
	ctob(r[2], x);
	send(&x, L3);
} //end of decoder 

char ftostr(float x, int n) {
	while(x>=10){
		x/=10;
	}
	x*=(10^(n-1)); 
	int y = (int)x;
	x-=y;
	x*=10;
	y = (int)x;
	return 48 + y;
}

void rnd (float f, char *c){
	int helper = 0;
	if (f>=0){
		helper = P1; //defined as pin
	}
	if (f>=10){
		helper = P2;
	}
	if (f>=100){
		helper = P3;
	}
	digitalWrite(helper, HIGH); 
	
	c[0] = ftostr(f, 0);
	c[1] = ftostr(f, 1);
	int temp = ftostr(f, 2);
	if (ftostr(f,3) >= 5){
		temp+=1;
	}
	c[2] = temp;
}

void ctob(char i, int *x){
	x[0] = i&0x01;
	x[1] = ((i>>1)&0x01);
	x[2] = ((i>>2)&0x01);
	x[3] = ((i>>3)&0x01);
	x[4] = ((i>>4)&0x01);
	x[5] = ((i>>5)&0x01);
	x[6] = ((i>>6)&0x01);
}

void send(int* t, int dsp){
	digitalWrite(dsp, LOW);
	digitalWrite(D, t[3]);
	digitalWrite(C, t[2]);
	digitalWrite(B, t[1]);
	digitalWrite(A, t[0]);
	delay(21);
	digitalWrite(dsp, HIGH);
}

//---------------------------

void main(){
	//...get float val
	decoder(val);
	//delay?
}
