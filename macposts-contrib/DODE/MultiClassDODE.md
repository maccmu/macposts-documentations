# MultiClassDODE Documentation

This document provides an overview of the classes and their functionalities in the `mcdode.py` file.

## Classes

### 1. `torch_pathflow_solver`

#### Description
This class is designed to solve path flow problems using PyTorch. It includes methods for initializing tensors, setting optimizers, computing gradients, and generating path flow tensors.

#### Methods
- `__init__(self, num_assign_interval, num_path, car_scale=1, truck_scale=0.1, use_file_as_init=None)`: Initializes the solver with the given parameters.
- `init_tensor(self, x: torch.Tensor)`: Initializes a tensor using Xavier uniform distribution.
- `initialize(self, use_file_as_init=None)`: Initializes the solver with optional file input.
- `get_log_f_tensor(self)`: Returns the log flow tensor.
- `get_log_f_numpy(self)`: Returns the log flow as a numpy array.
- `add_pathflow(self, num_path_add: int)`: Adds path flow.
- `generate_pathflow_tensor(self)`: Generates path flow tensor using softplus activation.
- `generate_pathflow_numpy(self, f_car: Union[torch.Tensor, None] = None, f_truck: Union[torch.Tensor, None] = None)`: Generates path flow as numpy arrays.
- `set_params_with_lr(self, car_step_size=1e-2, truck_step_size=1e-3)`: Sets parameters with learning rates.
- `set_optimizer(self, algo='NAdam')`: Sets the optimizer.
- `compute_gradient(self, f_car: torch.Tensor, f_truck: torch.Tensor, f_car_grad: np.ndarray, f_truck_grad: np.ndarray, l2_coeff: float = 1e-5)`: Computes gradients.
- `set_scheduler(self)`: Sets the learning rate scheduler.

### 2. `MCDODE`

#### Description
This class is designed for multi-class dynamic origin-destination estimation (DODE). It includes methods for initializing, running simulations, adding data, and estimating path flows.

#### Methods
- `__init__(self, nb, config, num_procs=1)`: Initializes the MCDODE with the given parameters.
- `get_registered_links_coverage_by_registered_paths(self, folder)`: Gets the coverage of registered links by registered paths.
- `check_registered_links_covered_by_registered_paths(self, folder, add=False)`: Checks if registered links are covered by registered paths.
- `generate_paths_to_cover_links(self, folder, links, max_iter)`: Generates paths to cover links.
- `_add_car_link_flow_data(self, link_flow_df_list)`: Adds car link flow data.
- `_add_truck_link_flow_data(self, link_flow_df_list)`: Adds truck link flow data.
- `_add_car_link_tt_data(self, link_spd_df_list)`: Adds car link travel time data.
- `_add_truck_link_tt_data(self, link_spd_df_list)`: Adds truck link travel time data.
- `_add_origin_vehicle_registration_data(self, origin_vehicle_registration_data_list)`: Adds origin vehicle registration data.
- `add_data(self, data_dict)`: Adds data to the MCDODE.
- `save_simulation_input_files(self, folder_path, f_car=None, f_truck=None)`: Saves simulation input files.
- `_run_simulation(self, f_car, f_truck, counter=0, show_loading=False)`: Runs the simulation.
- `get_dar3(self, dta, f_car, f_truck, engine='pyarrow')`: Gets the DAR matrix.
- `get_dar2(self, dta, f_car, f_truck)`: Gets the DAR matrix (alternative method).
- `get_dar(self, dta, f_car, f_truck)`: Gets the DAR matrix.
- `_massage_raw_dar(self, raw_dar, ass_freq, f, num_assign_interval, paths_list, observed_links, num_procs=5)`: Processes raw DAR data.
- `get_ltg(self, dta)`: Gets the LTG matrix.
- `init_demand_vector(self, num_assign_interval, num_col, scale=1)`: Initializes the demand vector.
- `init_path_flow(self, car_scale=1, truck_scale=0.1)`: Initializes path flow.
- `estimate_path_flow_scale(self)`: Estimates the path flow scale.
- `compute_path_flow_grad_and_loss(self, one_data_dict, f_car, f_truck, counter=0)`: Computes path flow gradient and loss.
- `_compute_count_loss_grad_on_car_link_flow(self, dta, one_data_dict)`: Computes count loss gradient on car link flow.
- `_compute_count_loss_grad_on_truck_link_flow(self, dta, one_data_dict)`: Computes count loss gradient on truck link flow.
- `_compute_tt_loss_grad_on_car_link_tt(self, dta, one_data_dict)`: Computes travel time loss gradient on car link travel time.
- `_compute_tt_loss_grad_on_truck_link_tt(self, dta, one_data_dict)`: Computes travel time loss gradient on truck link travel time.
- `_compute_tt_loss_grad_on_car_link_flow(self, dta, tt_loss_grad_on_car_link_tt)`: Computes travel time loss gradient on car link flow.
- `_compute_tt_loss_grad_on_truck_link_flow(self, dta, tt_loss_grad_on_truck_link_tt)`: Computes travel time loss gradient on truck link flow.
- `_compute_link_tt_grad_on_link_flow_car(self, dta)`: Computes link travel time gradient on link flow for cars.
- `_compute_link_tt_grad_on_link_flow_truck(self, dta)`: Computes link travel time gradient on link flow for trucks.
- `_get_flow_from_cc(self, timestamp, cc)`: Gets flow from cumulative count.
- `_compute_link_tt_grad_on_path_flow_car(self, dta)`: Computes link travel time gradient on path flow for cars.
- `_compute_link_tt_grad_on_path_flow_truck(self, dta)`: Computes link travel time gradient on path flow for trucks.
- `_massage_raw_ltg(self, raw_ltg, ass_freq, num_assign_interval, paths_list, observed_links, num_procs=5)`: Processes raw LTG data.
- `_compute_grad_on_origin_vehicle_registration_data(self, one_data_dict, f_car, f_truck)`: Computes gradient on origin vehicle registration data.
- `_compute_grad_on_origin_vehicle_registration_data2(self, one_data_dict, f_car, f_truck)`: Computes gradient on origin vehicle registration data (alternative method).
- `_get_one_data(self, j)`: Gets one data point.
- `aggregate_f(self, f_car, f_truck)`: Aggregates path flow.
- `_get_loss(self, one_data_dict, dta, f_car, f_truck)`: Computes loss.
- `estimate_path_flow_pytorch(self, car_step_size=0.1, truck_step_size=0.1, link_car_flow_weight=1, link_truck_flow_weight=1, link_car_tt_weight=1, link_truck_tt_weight=1, origin_vehicle_registration_weight=1, max_epoch=10, algo='NAdam', l2_coeff=1e-6, car_init_scale=10, truck_init_scale=1, store_folder=None, use_file_as_init=None, starting_epoch=0, column_generation=False)`: Estimates path flow using PyTorch.
- `estimate_path_flow_pytorch2(self, car_step_size=0.1, truck_step_size=0.1, link_car_flow_weight=1, link_truck_flow_weight=1, link_car_tt_weight=1, link_truck_tt_weight=1, origin_vehicle_registration_weight=1, max_epoch=10, algo='NAdam', normalized_by_scale=True, car_init_scale=10, truck_init_scale=1, store_folder=None, use_file_as_init=None, starting_epoch=0, column_generation=False)`: Estimates path flow using PyTorch (alternative method).
- `estimate_path_flow(self, car_step_size=0.1, truck_step_size=0.1, link_car_flow_weight=1, link_truck_flow_weight=1, link_car_tt_weight=1, link_truck_tt_weight=1, origin_vehicle_registration_weight=1e-6, max_epoch=10, car_init_scale=10, truck_init_scale=1, store_folder=None, use_file_as_init=None, adagrad=False, starting_epoch=0)`: Estimates path flow.
- `estimate_path_flow_gd(self, car_step_size=0.1, truck_step_size=0.1, max_epoch=10, link_car_flow_weight=1, link_truck_flow_weight=1, link_car_tt_weight=1, link_truck_tt_weight=1, car_init_scale=10, truck_init_scale=1, store_folder=None, use_file_as_init=None, adagrad=False, starting_epoch=0)`: Estimates path flow using gradient descent.
- `compute_path_flow_grad_and_loss_mpwrapper(self, one_data_dict, f_car, f_truck, j, output)`: Computes path flow gradient and loss (multiprocessing wrapper).
- `estimate_path_flow_mp(self, car_step_size=0.1, truck_step_size=0.1, link_car_flow_weight=1, link_truck_flow_weight=1, link_car_tt_weight=1, link_truck_tt_weight=1, max_epoch=10, car_init_scale=10, truck_init_scale=1, store_folder=None, use_file_as_init=None, adagrad=False, starting_epoch=0, n_process=4, record_time=False)`: Estimates path flow using multiprocessing.
- `generate_route_choice(self)`: Generates route choice.
- `print_separate_accuracy(self, loss_dict)`: Prints separate accuracy.
- `update_path_table(self, dta)`: Updates the path table.
- `update_path_flow(self, f_car, f_truck, car_init_scale=1, truck_init_scale=0.1)`: Updates path flow.

### 3. `mcSPSA`

#### Description
This class implements the Simultaneous Perturbation Stochastic Approximation (SPSA) algorithm for path flow estimation.

#### Methods
- `__init__(self, nb, config)`: Initializes the mcSPSA with the given parameters.
- `_add_car_link_flow_data(self, link_flow_df_list)`: Adds car link flow data.
- `_add_truck_link_flow_data(self, link_flow_df_list)`: Adds truck link flow data.
- `_add_car_link_tt_data(self, link_spd_df_list)`: Adds car link travel time data.
- `_add_truck_link_tt_data(self, link_spd_df_list)`: Adds truck link travel time data.
- `add_data(self, data_dict)`: Adds data to the mcSPSA.
- `save_simulation_input_files(self, folder_path, f_car=None, f_truck=None)`: Saves simulation input files.
- `_run_simulation(self, f_car, f_truck, counter=0, show_loading=False)`: Runs the simulation.
- `init_path_flow(self, car_scale=1, truck_scale=0.1)`: Initializes path flow.
- `_get_one_data(self, j)`: Gets one data point.
- `_get_loss(self, one_data_dict, dta)`: Computes loss.
- `compute_path_flow_grad_and_loss(self, one_data_dict, f_car, f_truck, delta_car_scale=0.1, delta_truck_scale=0.01)`: Computes path flow gradient and loss.
- `estimate_path_flow(self, car_step_size=0.1, truck_step_size=0.1, max_epoch=10, car_init_scale=10, truck_init_scale=1, store_folder=None, use_file_as_init=None, adagrad=False, delta_car_scale=0.1, delta_truck_scale=0.01)`: Estimates path flow.
- `print_separate_accuracy(self, loss_dict)`: Prints separate accuracy.

### 4. `PostProcessing`

#### Description
This class provides post-processing functionalities for the DODE results, including plotting and calculating statistics.

#### Methods
- `__init__(self, dode, dta=None, f_car=None, f_truck=None, estimated_car_count=None, estimated_truck_count=None, estimated_car_cost=None, estimated_truck_cost=None, link_length=None, estimated_origin_demand=None, result_folder=None)`: Initializes the PostProcessing with the given parameters.
- `plot_total_loss(self, loss_list, fig_name='total_loss_pathflow.png')`: Plots the total loss.
- `plot_breakdown_loss(self, loss_list, fig_name='breakdown_loss_pathflow.png')`: Plots the breakdown of the loss.
- `get_one_data(self, start_intervals, end_intervals, j=0)`: Gets one data point.
- `cal_r2_count(self)`: Calculates the R2 score for counts.
- `scatter_plot_count(self, fig_name='link_flow_scatterplot_pathflow.png')`: Creates a scatter plot for counts.
- `scatter_plot_count_by_link(self, link_list=None, fig_name='link_flow_scatterplot_pathflow_by_link')`: Creates a scatter plot for counts by link.
- `cal_r2_cost(self)`: Calculates the R2 score for costs.
- `cal_r2_speed(self)`: Calculates the R2 score for speeds.
- `scatter_plot_cost(self, fig_name='link_cost_scatterplot_pathflow.png')`: Creates a scatter plot for costs.
- `scatter_plot_cost_by_link(self, link_list=None, fig_name='link_cost_scatterplot_pathflow_by_link')`: Creates a scatter plot for costs by link.
- `scatter_plot_speed(self, fig_name='link_speed_scatterplot_pathflow.png')`: Creates a scatter plot for speeds.
