# jetson_power
Energy consumption estimation utilities for Jetson-based platforms


This repository contains a utility for measuring energy consumption when running various programs in NVIDIA Jetson-based platforms.
Currently TX-2, NX, and AGX are supported.

## Usage
If you want to measure the energy consumption of a program, you can directly run the utility providing the command that you want to measure:
```bash
./p_est program_to_run
```
You can test the utility using a stress test (make sure you have installed stress - `apt install stress`), e.g., 
```bash
./p_est stress --cpu 6 -t 5
```
You can also run a GPU-based test using CUDA examples:
```bash
sudo make  /usr/local/cuda-10.2/samples/0_Simple/matrixMul/matrixMul
./p_est.py /usr/local/cuda-10.2/samples/0_Simple/matrixMul/matrixMul -wA=9200 -hA=320 -wB=640 -hB=9200
```
You can play around `jetson_clocks.sh` and see the consumption indeed increasing.


## Interfacing with Python for more precise measurements 
Using the utility from the command line can include initialization cost in the power consumption. 
Even though this can be estimated and then subtracted, we also provide a simple Python API:
```python
from p_est import PowerEstimator
from time import sleep

def my_fun():
    for i in range(5):
        sleep(1)
        print('sleeping')


p_est = PowerEstimator()
total_energy, total_energy_over_idle, total_time = p_est.estimate_fn_power(my_fun)
```
You can use the `PowerEstimator` class to directly measure the energy consumption of any function.


# Things to consider
- Currently, the tool has been tested only on AGX. Testing is pending on NX and TX2.
- If sensors report overlapping power measurements, then the tools might overestimate power usage.
- Power usage is estimated solely using the sensors provided by Jetsons. This usually underestimates the total power.
- You can consider increasing `sampling rate` in order to have more precise measurements.

