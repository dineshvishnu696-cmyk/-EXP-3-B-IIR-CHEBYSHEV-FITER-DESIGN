# EXP 3 : IIR-CHEBYSHEV-FITER-DESIGN

## AIM: 

 To design an IIR Chebyshev filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 
```
clc ;
close ;
wp=input('Enter the pass band frequency (Radians )= ' );
ws=input('Enter the stop band frequency (Radians )= ' );
alphap=input( ' Enter the pass band attenuation (dB)=' );
alphas=input( ' Enter the stop band attenuation(dB)=' );
T=input('Enter the Value of sampling Time=');
omegap=(2/T)*tan(wp/2);
disp(omegap,'omegap=');
omegas=(2/T)*tan(ws/2);
N=acosh(sqrt(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1)))/(acosh(omegas/omegap));
disp(N,'N=');
N=ceil(N);
disp(N,'Round off value of N=');
omegac=omegap/(((10^(0.1*alphap)) -1)^(1/(2* N)));
disp(omegac,'omegac=');
Epsilon = sqrt ((10^(0.1*alphap))-1);
disp(Epsilon,'Epsilon=');
[pols ,gn] = zpch1(N, Epsilon,omegap );
disp(gn,'Gain');
disp(pols,'Poles');
hs=poly(gn,'s','coeff')/real(poly(pols,'s'));
disp(hs,'Analog Low pass Chebyshev Filter Transfer function');
z=poly(0,'z');
Hz=horner(hs,(2/ T)*((z -1)/(z+1)));
disp(Hz,'Digital LPF Transfer function H(Z)=');
HW=frmag(Hz,512); 
w=0:%pi/511:%pi;
plot(w/%pi,abs(HW));
xlabel(' Normalized Digital Frequency w');
ylabel('Magnitude ');
title(' Frequency Response of Chebyshev IIR LPF');

```


## PROGRAM (HPF): 
```
clc;
clear;
close;

// Input parameters
wp = input('Enter the pass band frequency (Radians) = ');
ws = input('Enter the stop band frequency (Radians) = ');
alphap = input('Enter the pass band attenuation (dB) = ');
alphas = input('Enter the stop band attenuation (dB) = ');
T = input('Enter the sampling time = ');

// Prewarp the digital frequencies
omegap = (2/T)*tan(wp/2);
disp(omegap, 'omegap=');
omegas = (2/T)*tan(ws/2);

// Filter order calculation using Chebyshev formula
N = acosh(sqrt(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1))) / acosh(omegas/omegap);
disp(N, 'N=');
N = ceil(N);
disp(N, 'Round off value of N=');

// Cutoff frequency
omegac = omegap / (((10^(0.1*alphap)) - 1)^(1/(2*N)));
disp(omegac, 'omegac=');

Epsilon = sqrt((10^(0.1*alphap)) - 1);
disp(Epsilon, 'Epsilon=');

// Get poles and gain for Chebyshev Type I lowpass prototype
[pols, gn] = zpch1(N, Epsilon, omegap);
disp(gn, 'Gain');
disp(pols, 'Poles');

// Analog lowpass transfer function
hs_lp = poly(gn, 's', 'coeff') / real(poly(pols, 's'));
disp(hs_lp, 'Analog Low pass Chebyshev Filter Transfer function');

// Frequency transform from lowpass to highpass: s -> omegac / s
s = poly(0, 's');
hs_hp = horner(hs_lp, omegac ./ s);
disp(hs_hp, 'Analog High pass Chebyshev Filter Transfer function');

// Bilinear transform to convert to digital filter
z = poly(0, 'z');
Hz = horner(hs_hp, (2 / T) * ((z - 1) / (z + 1)));
disp(Hz, 'Digital HPF Transfer function H(Z)=');

// Frequency response
HW = frmag(Hz, 512);
w = 0 : %pi / 511 : %pi;
plot(w / %pi, abs(HW));
xlabel('Normalized Digital Frequency w');
ylabel('Magnitude');
title('Frequency Response of Chebyshev IIR Highpass Filter');

```


## OUTPUT (LPF) : 

<img width="1920" height="1080" alt="Screenshot 2025-09-22 152037" src="https://github.com/user-attachments/assets/8410d3fc-ecb2-4956-a7e2-9ed965cb2cc3" />


## OUTPUT (HPF) : 

<img width="1920" height="1080" alt="Screenshot 2025-09-22 174500" src="https://github.com/user-attachments/assets/2d24cd93-336f-4ca8-9f50-1b2260827aae" />


## RESULT: 

The design of an IIR Chebyshev filter  using SCILAB is sucessfully completed. 
