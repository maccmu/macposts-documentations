# DODE Class Documentation

## DODE Class

The `DODE` class is designed to handle dynamic origin-destination estimation (DODE) using various data inputs and simulation methods.

### Initialization

```python
def __init__(self, nb, config):
```
- **Parameters:**
    - `nb`: Network builder object.
    - `config`: Configuration dictionary.

### Methods

#### `_add_link_flow_data`

```python
def _add_link_flow_data(self, link_flow_df_list):
```
- Adds link flow data to the data dictionary.

#### `_add_link_tt_data`

```python
def _add_link_tt_data(self, link_spd_df_list):
```
- Adds link travel time data to the data dictionary.

#### `add_data`

```python
def add_data(self, data_dict):
```
- Adds data to the data dictionary.

#### `_run_simulation`

```python
def _run_simulation(self, f, counter):
```
- Runs the simulation and returns the simulation object.

#### `get_dar`

```python
def get_dar(self, dta, f):
```
- Gets the dynamic assignment ratio (DAR) matrix.

#### `get_dar2`

```python
def get_dar2(self, dta, f):
```
- Gets the complete DAR matrix.

#### `_massage_raw_dar`

```python
def _massage_raw_dar(self, raw_dar, ass_freq, f, num_assign_interval):
```
- Processes the raw DAR matrix.

#### `init_path_flow`

```python
def init_path_flow(self, init_scale):
```
- Initializes the path flow.

#### `compute_path_flow_grad_and_loss`

```python
def compute_path_flow_grad_and_loss(self, one_data_dict, f, counter=0):
```
- Computes the gradient and loss for path flow.

#### `compute_path_flow_grad_and_loss_mpwrapper`

```python
def compute_path_flow_grad_and_loss_mpwrapper(self, one_data_dict, f, j, output):
```
- Wrapper for computing gradient and loss using multiprocessing.

#### `_compute_grad_on_link_flow`

```python
def _compute_grad_on_link_flow(self, dta, link_flow_array):
```
- Computes the gradient on link flow.

#### `_compute_grad_on_link_tt`

```python
def _compute_grad_on_link_tt(self, dta, link_flow_array):
```
- Computes the gradient on link travel time.

#### `_get_one_data`

```python
def _get_one_data(self, j):
```
- Gets one data dictionary.

#### `_get_loss`

```python
def _get_loss(self, one_data_dict, dta):
```
- Computes the loss.

#### `estimate_path_flow_pytorch`

```python
def estimate_path_flow_pytorch(self, init_scale=0.1, step_size=0.1, max_epoch=100):
```
- Estimates path flow using PyTorch.

#### `estimate_path_flow`

```python
def estimate_path_flow(self, init_scale=0.1, step_size=0.1, max_epoch=100, adagrad=False):
```
- Estimates path flow.

#### `estimate_path_flow_mp_pytorch`

```python
def estimate_path_flow_mp_pytorch(self, init_scale=0.1, step_size=0.1, max_epoch=100, n_process=4):
```
- Estimates path flow using PyTorch with multiprocessing.

#### `estimate_path_flow_mp`

```python
def estimate_path_flow_mp(self, init_scale=0.1, step_size=0.1, max_epoch=100, adagrad=False, n_process=4):
```
- Estimates path flow using multiprocessing.

#### `generate_route_choice`

```python
def generate_route_choice(self):
```
- Placeholder for generating route choice.

## PDODE Class

The `PDODE` class extends the `DODE` class to handle probabilistic dynamic origin-destination estimation.

### Initialization

```python
def __init__(self, nb, config):
```
- **Parameters:**
    - `nb`: Network builder object.
    - `config`: Configuration dictionary.

### Methods

#### `init_path_distribution`

```python
def init_path_distribution(self, flow_mean_scale=1, flow_variance_scale=1):
```
- Initializes the path distribution.

#### `generate_standard_normal`

```python
def generate_standard_normal(self):
```
- Generates a standard normal distribution.

#### `dod_forward`

```python
def dod_forward(self, stdnorm, f_u, f_sigma):
```
- Forward pass for DODE.

#### `dod_backward`

```python
def dod_backward(self, stdnorm, f_u, f_sigma, g):
```
- Backward pass for DODE.

#### `estimate_path_flow_pytorch`

```python
def estimate_path_flow_pytorch(self, init_scale=0.1, step_size=0.1, max_epoch=100):
```
- Estimates path flow using PyTorch.

#### `estimate_path_flow`

```python
def estimate_path_flow(self, init_scale=0.1, step_size=0.1, max_epoch=100, adagrad=False):
```
- Estimates path flow.
