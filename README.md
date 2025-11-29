# FREQUENCY-DIVISION-MULTIPLEXING-FDM---SIMULATION-USING-SCILAB

# AIM

To generate multiple message signals, multiplex them using AM-FDM, and recover each channel using bandpass filtering and coherent demodulation in Scilab.

# APPARATUS REQUIRED

1. Computer with Scilab
2. Scilab script (.sci file)
3. Basic DSP knowledge (filters, convolution, FFT)

# ALGORITHM

1. Generate message signals ( m_i(t) ).
2. Multiply each with its carrier to form AM-DSB.
3. Sum all channels → FDM composite.
4. For each channel:
   a. Band-pass filter around carrier.
   b. Multiply with same carrier (coherent detection).
   c. Low-pass filter to recover baseband.
5. Plot message, FDM, recovered outputs.

# PROCEDURE 

1. Define sampling rate, time vector, message and carrier frequencies.
2. Generate sinusoidal messages and modulate them with cosine carriers.
3. Add all modulated signals to form the composite FDM signal.
4. Design FIR bandpass filter to isolate each channel.
5. Demodulate by mixing with original carrier and low-pass filtering.
6. Plot original messages, FDM signal and recovered signals.
# PROGRM
~~~
clc; clear; close;
fs=80000; N=floor(0.05*fs); t=(0:N-1)/fs;
fm=[405 415 425 435 445 455];
fc=[4050 4150 4250 4350 4450 4550];
Am=[5.1 5.2 5.3 5.4 5.5 5.6];
Ac=[10.2 10.4 10.6 10.8 11 11.2];
num=length(fm);
for i=1:num
    m(i,:)=Am(i)*sin(2*%pi*fm(i)*t);
end
fdm=zeros(1,N);
for i=1:num
    fdm = fdm + Ac(i)*cos(2*%pi*fc(i)*t).*m(i,:);
end
function h=FIR(fc1,fc2,fs,M,mode)
    n=-M:M; L=length(n);
    x1=2*fc1*n/fs; x2=2*fc2*n/fs;
    s1=ones(1,L); s2=ones(1,L);
    for k=1:L
        if x1(k)<>0 then s1(k)=sin(%pi*x1(k))/(%pi*x1(k)); end
        if x2(k)<>0 then s2(k)=sin(%pi*x2(k))/(%pi*x2(k)); end
    end
    lp1=(2*fc1/fs)*s1; lp2=(2*fc2/fs)*s2;
    w=(0.54-0.46*cos(2*%pi*(n+M)/(2*M)));
    if mode==1 then h=lp1.*w;
    else h=(lp2-lp1).*w; end
    h=h/sum(h);
endfunction
M=300;
for i=1:num
    bp=FIR(fc(i)-600,fc(i)+600,fs,M,2);
    iso=conv(fdm,bp,"same");
    mix=iso.*cos(2*%pi*fc(i)*t);
    lp=FIR(800,0,fs,M,1);
    demod(i,:)= (2/Ac(i))*conv(mix,lp,"same");
end
scf(1); clf;
for i=1:num subplot(num,1,i); plot(t,m(i,:)); end
scf(2); clf; plot(t,fdm);
scf(3); clf;
for i=1:num subplot(num,1,i); plot(t,demod(i,:)); end


~~~
# OUTPUT
<img width="750" height="700" alt="image" src="https://github.com/user-attachments/assets/663af5c2-99a7-4a89-8402-4f59b17390f1" />

<img width="756" height="690" alt="image" src="https://github.com/user-attachments/assets/e021528c-9a68-49d8-9c73-36a96140c293" />


<img width="715" height="705" alt="image" src="https://github.com/user-attachments/assets/6a4c2146-9d98-4efd-9600-e2b421f1a410" />


# MANUAL CALCULATION

![WhatsApp Image 2025-11-29 at 13 29 02_eb5a80f1](https://github.com/user-attachments/assets/64734ac2-e371-4bd4-98f3-12e0391808e6)


![WhatsApp Image 2025-11-29 at 13 29 03_b59a1c7d](https://github.com/user-attachments/assets/ece05fa7-2341-4d8b-8e50-1403494e1ac6)

# RESULT 

All message frequencies (400 - 500Hz) were successfully multiplexed and individually recovered with minimal distortion using bandpass isolation and coherent demodulation.
