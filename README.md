# Amplitud--modulada
Implementación de un sistema AM incluyendo: - Modulación básica - Ruido AWGN - Sobremodulación - Atenuación
clear; clc; clf;

// -------------------------------
// 1. Parámetros de simulación
// -------------------------------
fs = 10000;             // Frecuencia de muestreo (Hz)
t = 0:1/fs:1;           // Vector de tiempo (1 segundo)

// Señal moduladora (mensaje) - tono de baja frecuencia
fm = 5;                 // Frecuencia del mensaje (Hz)
Am = 1;                 // Amplitud del mensaje
m = Am * cos(2*%pi*fm*t);   // Señal moduladora

// Señal portadora
fc = 100;               // Frecuencia portadora (Hz)
Ac = 1;                 // Amplitud portadora
c = Ac * cos(2*%pi*fc*t);   // Señal portadora

// Índice de modulación (para evitar sobremodulación)
indice_mod = 0.8;       // 0 < indice_mod <= 1

// -------------------------------
// 2. Modulación AM
// -------------------------------
s_am = (1 + indice_mod * m) .* c;   // Señal AM

// -------------------------------
// 3. Visualización en tiempo y frecuencia
// -------------------------------

// Figura 1: Señal moduladora
figure(1);
subplot(2,2,1);
plot(t(1:500), m(1:500));
title('Señal Moduladora (Mensaje)');
xlabel('Tiempo (s)'); ylabel('Amplitud');
xgrid;

// Figura 1: Señal portadora
subplot(2,2,2);
plot(t(1:500), c(1:500));
title('Señal Portadora');
xlabel('Tiempo (s)'); ylabel('Amplitud');
xgrid;

// Figura 1: Señal AM modulada
subplot(2,2,3);
plot(t(1:500), s_am(1:500));
title('Señal AM Modulada');
xlabel('Tiempo (s)'); ylabel('Amplitud');
xgrid;

// Espectro de frecuencias (FFT)
n = length(t);
f = (0:n/2-1)*fs/n;

S_am = fft(s_am);
S_am_mag = abs(S_am(1:n/2)) / n;

subplot(2,2,4);
plot(f, S_am_mag);
title('Espectro de la Señal AM');
xlabel('Frecuencia (Hz)'); ylabel('Magnitud');
xgrid;

// -------------------------------
// 4. Introducción de ruido
// -------------------------------
SNR_dB = 10;            // Relación señal-ruido en dB
ruido = (1/(10^(SNR_dB/20))) * rand(1, length(t), 'normal');
s_am_ruidosa = s_am + ruido;

// Figura 2: Señal AM con ruido
figure(2);
subplot(2,1,1);
plot(t(1:500), s_am_ruidosa(1:500));
title('Señal AM con Ruido (SNR = 10 dB)');
xlabel('Tiempo (s)'); ylabel('Amplitud');
xgrid;

// Espectro con ruido
S_am_ruidosa = fft(s_am_ruidosa);
S_am_ruidosa_mag = abs(S_am_ruidosa(1:n/2)) / n;

subplot(2,1,2);
plot(f, S_am_ruidosa_mag);
title('Espectro con Ruido');
xlabel('Frecuencia (Hz)'); ylabel('Magnitud');
xgrid;

// -------------------------------
// 5. Distorsión por sobremodulación
// -------------------------------
indice_mod_alto = 1.5;   // Índice > 1 produce sobremodulación
s_am_distorsion = (1 + indice_mod_alto * m) .* c;

figure(3);
subplot(2,1,1);
plot(t(1:500), s_am_distorsion(1:500));
title('Sobremodulación (índice = 1.5) - Distorsión');
xlabel('Tiempo (s)'); ylabel('Amplitud');
xgrid;

// Espectro con distorsión
S_am_dist = fft(s_am_distorsion);
S_am_dist_mag = abs(S_am_dist(1:n/2)) / n;

subplot(2,1,2);
plot(f, S_am_dist_mag);
title('Espectro con Sobremodulación');
xlabel('Frecuencia (Hz)'); ylabel('Magnitud');
xgrid;

// -------------------------------
// 6. Atenuación de la señal
// -------------------------------
atenuacion = 0.3;        // Factor de atenuación
s_am_atenuada = atenuacion * s_am;

figure(4);
subplot(2,1,1);
plot(t(1:500), s_am_atenuada(1:500));
title('Señal AM Atenuada (30% de amplitud original)');
xlabel('Tiempo (s)'); ylabel('Amplitud');
xgrid;

// Espectro con atenuación
S_am_aten = fft(s_am_atenuada);
S_am_aten_mag = abs(S_am_aten(1:n/2)) / n;

subplot(2,1,2);
plot(f, S_am_aten_mag);
title('Espectro con Atenuación');
xlabel('Frecuencia (Hz)'); ylabel('Magnitud');
xgrid;

// -------------------------------
// 7. Conclusión en consola
// -------------------------------
disp('==========================================');
disp('Análisis de Modulación AM en Scilab');
disp('==========================================');
disp('- Señal AM generada correctamente');
disp('- Ruido añadido (SNR = 10 dB) degrada la señal');
disp('- Sobremodulación (índice > 1) produce distorsión');
disp('- Atenuación reduce amplitud pero mantiene forma');
disp('Ver gráficas para análisis detallado.');
