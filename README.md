## AMPLITUDE SHIFT KEYING AND FREQUENCY SHIFT KEYING
### AIM
To write a simple Python program for the modulation and demodulation of ASK and FSK.
### TOOLS REQUIRED
Python IDE with Numpy and Scipy Libraries or Colab
### PROGRAM
#### ASK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

def butter_lowpass_filter(data, cutoff, fs, order=5):
    b, a = butter(order, cutoff/(0.5*fs), btype='low')
    return lfilter(b, a, data)

fs = 1000
f_carrier = 50
bit_rate = 10
T = 1

t = np.linspace(0, T, int(fs*T), endpoint=False)

bits = np.random.randint(0, 2, bit_rate)
bit_duration = fs // bit_rate
message_signal = np.repeat(bits, bit_duration)

carrier = np.sin(2*np.pi*f_carrier*t)

ask_signal = message_signal * carrier
demodulated = ask_signal * carrier
filtered_signal = butter_lowpass_filter(demodulated, f_carrier, fs)

decoded_bits = (filtered_signal[::bit_duration] > 0.25).astype(int)

plt.figure(figsize=(10,8))

plt.subplot(4,1,1)
plt.plot(t, message_signal)
plt.title("Message Signal")
plt.grid()

plt.subplot(4,1,2)
plt.plot(t, carrier)
plt.title("Carrier Signal")
plt.grid()

plt.subplot(4,1,3)
plt.plot(t, ask_signal)
plt.title("ASK Modulated Signal")
plt.grid()

plt.subplot(4,1,4)
plt.step(range(len(decoded_bits)), decoded_bits)
plt.title("Demodulated Bits")
plt.grid()

plt.tight_layout()
plt.show()

```
#### FSK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

def lpf(x, fc, fs):
    b, a = butter(4, fc/(0.5*fs))
    return lfilter(b, a, x)

fs = 1000
f1 = 30
f2 = 70
bit_rate = 10
T = 1

t = np.linspace(0, T, fs)

bits = np.random.randint(0, 2, bit_rate)
bit_duration = fs // bit_rate
msg = np.repeat(bits, bit_duration)

carrier_f1 = np.sin(2*np.pi*f1*t)
carrier_f2 = np.sin(2*np.pi*f2*t)

# BFSK Modulation
fsk_signal = np.zeros_like(t)
for i, bit in enumerate(bits):
    s, e = i*bit_duration, (i+1)*bit_duration
    fsk_signal[s:e] = np.sin(2*np.pi*(f2 if bit else f1)*t[s:e])

# Coherent Demodulation
corr_f1 = lpf(fsk_signal * carrier_f1, f2, fs)
corr_f2 = lpf(fsk_signal * carrier_f2, f2, fs)

decoded_bits = []
for i in range(bit_rate):
    s, e = i*bit_duration, (i+1)*bit_duration
    decoded_bits.append(
        1 if np.sum(corr_f2[s:e]**2) > np.sum(corr_f1[s:e]**2) else 0
    )

demodulated_signal = np.repeat(decoded_bits, bit_duration)

# Plots
plt.figure(figsize=(10,8))

plt.subplot(5,1,1)
plt.plot(t, msg); plt.title("Message Signal"); plt.grid()

plt.subplot(5,1,2)
plt.plot(t, carrier_f1); plt.title("Carrier Signal (f1)"); plt.grid()

plt.subplot(5,1,3)
plt.plot(t, carrier_f2); plt.title("Carrier Signal (f2)"); plt.grid()

plt.subplot(5,1,4)
plt.plot(t, fsk_signal); plt.title("FSK Modulated Signal"); plt.grid()

plt.subplot(5,1,5)
plt.plot(t, demodulated_signal); plt.title("Demodulated Signal"); plt.grid()

plt.tight_layout()
plt.show()
```
### OUTPUT
#### ASK
<img width="600" height="600" alt="image" src="https://github.com/user-attachments/assets/1e3dbb47-b696-4254-8cbf-e417f3acdc87" />

#### FSK
<img width="600" height="595" alt="image" src="https://github.com/user-attachments/assets/6762ea50-8656-477a-a07a-277aff294742" />


### RESULT
Thus, the Python program for the modulation and demodulation of ASK and FSK has been successfully simulated and verified.
