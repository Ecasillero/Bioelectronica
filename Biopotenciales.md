````
clc;
clear;

% Frecuencia de muestreo
Fs = 500; % en Hz

% Cargar la señal ECG desde un archivo .mat
ekg = load('test01_00sm (1).mat');

% Obtener la señal ECG y normalizarla (ajuste según el archivo)
ekgsignal = (ekg.val - 1024) / 200;

% Crear el eje de tiempo en segundos
t = (0:length(ekgsignal)-1) / Fs;

% Graficar la señal ECG en el dominio del tiempo
figure;
plot(t, ekgsignal);
title('Señal ECG en el Dominio del Tiempo');
xlabel('Tiempo (s)');
ylabel('Amplitud');
grid on;

% -------------------------------
% Dominio de la Frecuencia con FFT
% -------------------------------

% Aplicar la Transformada de Fourier
L = length(ekgsignal);               % Longitud de la señal
ekg_fft = fft(ekgsignal);             % Transformada de Fourier de la señal
P2 = abs(ekg_fft / L);                % Magnitud de la FFT
P1 = P2(1:L/2+1);                     % Tomar solo la mitad positiva del espectro
P1(2:end-1) = 2 * P1(2:end-1);        % Ajustar magnitudes

% Crear el eje de frecuencias
f = Fs * (0:(L/2)) / L;

% Graficar la señal ECG en el dominio de la frecuencia
figure;
plot(f, P1);
title('Señal ECG en el Dominio de la Frecuencia (FFT)');
xlabel('Frecuencia (Hz)');
ylabel('Amplitud');
grid on;

% -------------------------------
% Espectrograma
% -------------------------------

% Parámetros del espectrograma
window = 256;                    % Tamaño de la ventana
noverlap = window / 2;           % Superposición entre ventanas
nfft = 512;                      % Número de puntos de la FFT para el espectrograma

% Graficar el espectrograma
figure;
spectrogram(ekgsignal, window, noverlap, nfft, Fs, 'yaxis');
title('Espectrograma de la Señal ECG');
colorbar;
ylabel('Frecuencia (Hz)');
xlabel('Tiempo (s)');
