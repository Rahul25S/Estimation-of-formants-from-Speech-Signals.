%formants extraction
[x,fs]=audioread('C:\Program Files\MATLAB\MATLAB Production Server\R2015a\bin\work\data\1-11.wav');
% plot waveform
t=(0:length(x)-1)/Fs;        % times of sampling instants
subplot(2,1,1);
plot(t,x);
legend('Waveform');
xlabel('Time (s)');
ylabel('Amplitude');
 
% get Linear prediction filter
 
ncoeff = round(Fs/1000) + 2;        % rule of thumb for formant estimation
%lpc  Linear Predictor Coefficients.
%A = lpc(X,N) finds the coefficients, A=[ 1 A(2) ... A(N+1) ], of an Nth
%order forward linear predictor.
a=lpc(x,ncoeff);
 
% plot frequency response
[h,f]=freqz(1,a,512,Fs);
subplot(2,1,2);
plot(f,20*log10(abs(h)+eps));
legend('LP Filter');
xlabel('Frequency (Hz)');
ylabel('Gain (dB)');
 
%To find the formant frequencies from the filter, we need to find the locations of the resonances that make up the filter.  
%This involves treating the filter coefficients as a polynomial and solving for the roots of the polynomial.  
 % find frequencies by root-solving
 
r=roots(a);                  % find roots of polynomial a
r=r(imag(r)>0.01);           % only look for roots >0Hz up to fs/2
 
ffreq=sort(atan2(imag(r),real(r))*Fs/(2*pi));
% convert to Hz and sort
 
for i=1:length(ffreq)
 
    fprintf('Formant %d Frequency %.1f\n',i,ffreq(i));
 
end
