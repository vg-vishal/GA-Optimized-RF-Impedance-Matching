clc; clear; close all;

% Parameters
Z0 = 50;                     
f = linspace(1e6,100e6,500); 
w = 2*pi*f;

% Biological load (same mismatched one)
ZL = 10 - 1j*30;             

% T-Network elements (tuned for matching)
C1 = 100e-12;   % 100 pF
L  = 250e-9;   % 250 nH
C2 = 50e-12;   % 50 pF

% Impedance of each element
ZC1 = 1./(1j*w*C1);
ZLnet = 1j*w*L;
ZC2 = 1./(1j*w*C2);

% Equivalent load with T-network
Zeq = ZC1 + ( (ZLnet + ZL).*ZC2 ) ./ (ZLnet + ZL + ZC2);

% Reflection coefficient
Gamma = (Zeq - Z0)./(Zeq + Z0);
S11_dB = 20*log10(abs(Gamma));

% Efficiency (delivered power %)
Efficiency = (1 - abs(Gamma).^2)*100;

% Plots
figure;
subplot(2,1,1);
plot(f/1e6, S11_dB,'g','LineWidth',2);
xlabel('Frequency (MHz)'); ylabel('S11 (dB)');
title('With T-Network Matching - Reflection Coefficient');
grid on;

subplot(2,1,2);
plot(f/1e6, Efficiency,'m','LineWidth',2);
xlabel('Frequency (MHz)'); ylabel('Efficiency (%)');
title('With T-Network Matching - Power Efficiency');
grid on;

% Display key values
[minS11, idx] = min(S11_dB);
fprintf('With Matching:\n');
fprintf('Resonant Frequency = %.2f MHz\n', f(idx)/1e6);
fprintf('Minimum S11 = %.2f dB\n', minS11);
fprintf('Efficiency at resonance = %.2f %%\n', Efficiency(idx));


