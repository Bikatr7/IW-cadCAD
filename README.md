# Index Wallets cadCAD Sim

This repo simulates a simple economy using index wallets, a proposed payment mechanism to incentivize voluntary taxation and public good funding.

Transactions take place over a grid-based network. Every agent in the grid is both a buyer and seller.

## Dependencies

The included `requirements.txt` file gives all of the dependencies required to run this code. They can be quickly installed all at once with the command

```
pip install -r requrements.txt
```

I _highly_ recommend using a [venv](https://docs.python.org/3/library/venv.html).

I strongly suspect conda or other python package managers will work fine, but I have only tested with pip.

## Executables

There are four executable files in this project. None take any command line arguments, and all should be run from the root level directory of this repo. Instead of command line arguments, there is usually a filepath somewhere obvious in the file giving the file it operates on.

`gen_sim.py`: Generates a new run of the economy simulation using `cadCAD`. This simulation is saved to disk under the directory `sim_results/<checked out commit hash>`. The saved file can be loaded using [pickle](https://docs.python.org/3/library/pickle.html). Currently, there are no safety / authorship checks on these files. In general, you should NOT use `pickle` to load files unless you know how it was created, or trust the source of the file. **Malicious files can cause arbitrary code execution!**

The object saved in this pickle file is the output of `cadCAD`s simulation execution. This is loosely a `DataFrame` like object, with one row for each timestep simulated. On each row, there is a column for every state and input variable in the simulation. See [cadCAD's demos](https://github.com/cadCAD-org/demos) for more information.

`view_sim.py`: Opens a very simple GUI to show how the valuation of agents changes over the duration of the simulation. For sims with more than 2 currencies, the methods used to display this information will likely need to be modified. There is a known issue where the slider to scroll through time has incorrect bounds. It may crash when attempting to view a timestep that does not exist in the viewed sim file.

`data_analysis.py`: Generates figures displaying (hopefully useful) summary information about a sim file. This is done in batches; the file will generate figures for an entire directory at once. Currently, it generates figures to show the amount of money each participant has at every timestep in the simulation, and the initial and final values that agents assign to every currency.

`gen_sim_forks.py`: For some sim file, it creates many new sim files with the same initial state except for a simple perturbation applied. This executable only generates the _initial timestep_ for these sims. By specifying a `conf_file` in `gen_sim.py`, it can be seen how these perturbations evolve through time.

## Parameters

The file `src/sim/params.py` contains many parameters that can adjust model execution. Below is a brief explanation of each of them.

`grid_size`: Number of agents on one side of the square grid
`edge_prob`: Probability that an edge exists between a node and any of its grid neighbors
`wraparound`: Whether edges are allowed to be generated between the left (top) and right (bottom) edges of the grid
`num_currencies`: How many currencies to include in the simulation
`demand_range`: Range to uniformly sample agents' demand for their vendors' goods from
`valuation_range`: Range to uniformly sample agents' starting valuations of each currency from
`price_range`: Range to uniformly sample the price that agents charge for their good from

`sim_timesteps`: Number of timesteps to run the simulation for
`initial_donation_reward_amount`: Reward size for 1 unit of donation utility, before any donations have been made
`donation_reward_mix`: Fraction of a currency reward that is returned in the original wallet

`eps`: Small value to check floating point equality