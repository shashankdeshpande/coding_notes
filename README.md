
## Contents
 - [Line Profiling](#Line_Profiling)
 - [Multithreading](#Multithreading)
 - [Multiprocessing](#Multiprocessing)
 

## [Line Profiling](#Line_Profiling)
```console
https://github.com/pyutils/line_profiler
$ pip install line_profiler
```
```python
from line_profiler import LineProfiler

def do_profile(follow=None):
    if not follow:
        follow = []

    def inner(func):
        def profiled_func(*args, **kwargs):
            try:
                profiler = LineProfiler()
                profiler.add_function(func)
                for f in follow:
                    profiler.add_function(f)
                profiler.enable_by_count()
                return func(*args, **kwargs)
            finally:
                profiler.print_stats()
        return profiled_func
    return inner

@do_profile()
def func_to_check(input):
    pass
```
## [Multithreading](#Multithreading)
```python
from concurrent.futures import ThreadPoolExecutor, as_completed

def func_to_apply(self, data_dict, res_list):
    ...
    res_list.append(...)

res_list = []

thread_executor= ThreadPoolExecutor(max_workers=8)
queue = [thread_executor.submit(self.func_to_apply, data_dict, res_list) for data_dict in input_data]

for each in as_completed(queue):
    each.result()

print(res_list)
```
## [Multiprocessing](#Multiprocessing)
```python
import multiprocessing as mp

def func_to_apply(self, data_dict, res_list):
    ...
    res_list.append(...)

manager = mp.Manager()
res_list = manager.list()
pool = mp.Pool(processes=8)

def callback_func():
    # usually to record progress
    ...

for data_dict in input_data:
    pool.apply_async(self.func_to_apply, args=[data_dict, res_list], callback=callback_func)

pool.close()
pool.join()

res_list = list(res_list)
```
