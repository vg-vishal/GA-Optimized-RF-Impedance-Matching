clc; clear; close all;

% Parameters
Z0 = 50;                           % Source impedance
f = linspace(1e6,100e6,1000);      % Frequency range (1–100 MHz)
w = 2*pi*f;

% Biological load (frequency-dependent, mismatched)
R = 10;                            % Resistive part
C_load = 100e-12;                  % 100 pF capacitor -> adds mismatch
ZL = R - 1j./(w*C_load);           % Complex load (depends on frequency)

% Reflection coefficient
Gamma = (ZL - Z0)./(ZL + Z0);
S11_dB = 20*log10(abs(Gamma));

% Efficiency (delivered power %)
Efficiency = (1 - abs(Gamma).^2)*100;

% Plots
figure;
subplot(2,1,1);
plot(f/1e6, S11_dB,'r','LineWidth',2);
xlabel('Frequency (MHz)'); ylabel('S11 (dB)');
title('Without Impedance Matching - Reflection Coefficient');
grid on;

subplot(2,1,2);
plot(f/1e6, Efficiency,'b','LineWidth',2);
xlabel('Frequency (MHz)'); ylabel('Efficiency (%)');
title('Without Impedance Matching - Power Efficiency');
grid on;

% Display key values
[minS11, idx] = min(S11_dB);
fprintf('Without Matching:\n');
fprintf('  Resonant Frequency = %.2f MHz\n', f(idx)/1e6);
fprintf('  Minimum S11 = %.2f dB\n', minS11);
fprintf('  Efficiency at resonance = %.2f %%\n', Efficiency(idx));


