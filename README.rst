Download the instrumented bicycle data from this project:

https://figshare.com/projects/Human_Control_Identification_During_Bicycling/22067

The only required file is the HDF5 file (the other three items are already
stored in the HDF5 file), but you can grab them all if you want to be complete.

Also download the bicycle parameter data from:

https://figshare.com/articles/Bicycle_Parameters/1198429

The files for any new riders are temporary stored in this repository. Copy this
data into the ``RawData`` folder for the particular rider.

You need to unzip these data files into a directory. It can look something like
this::

   data/
     bicycle-parameters/
       bicycles/
         Rigid/
       riders/
         Jason/
           RawData/
         Mont/
           RawData/
         Scott/
           RawData/
         Vien/
           RawData/
     davis-instrumented-bicycle/
       calibrations/
       runs/
       instrumented-bicycle-data.h5
       data-corruption.csv

Now, deposit any new calibration and run mat files from the bicycle's computer
into the ``calibrations`` and ``runs`` directories, respectively.

Navigate to this repository and you can create a conda environment with the
correct software versions using the ``environment.yml`` file. The
BicycleDataProcessor software will need to be installed via pip because of new
modifications.

::

   $ conda env create -f environment.yml
   $ source activate bikehandling  # on windows just "activate bikehandling"
   (bikehandling)$ pip install git+https://github.com/moorepants/BicycleDataProcessor.git@kresie-updates

Create a ``bdp-defaults.cfg`` file in this directory (do not commit it to the
repo) that points to all of the data locations. This is the one I use::

   [data]
   base = /home/moorepants/Data/davis-instrumented-bicycle
   pathToDatabase = %(base)s/instrumented-bicycle-data.h5
   pathToCorruption  = %(base)s/data-corruption.csv
   pathToRunMat = %(base)s/runs
   pathToCalibMat = %(base)s/calib
   pathToRunH5 = %(base)s/runs/h5
   pathToCalibH5 = %(base)s/calib/h5
   pathToParameters = /home/moorepants/Data/bicycle-parameters

Modify this to have the correct paths for your computer. You'll need to make
sure to use paths like ``C:\`` on Windows.

Now you can load the database and add the new runs. Open IPython to do this::

   (bikehandling)$ ipython
   In [1]: import bicycledataprocessor as bdp
   In [2]: dataset = bdp.DataSet()
   In [3]: dataset.fill_all_tables()

Once the new runs have been appended to the database you can load the runs and
check out the data, for example::

   In [4]: run = bdp.Run('00814', filter=10.0)
   In [5]: import matplotlib.pyplot as plt
   In [6]: run.plot('SteerAngle', 'RollAngle')
   In [7]: plt.show()
   In [8]: run.plot_wheel_contact()
   In [9]: plt.show()
