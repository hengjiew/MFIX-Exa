.. _Chap:InputsPlotfiles:

Plotfiles and Other Output
==========================

The following inputs must be preceded by "amr" and control frequency and naming of plotfile generation as well
as whether the EB geometry or level set should be written out, and if the particles should be written out in Ascii
format (for debugging).

+----------------------+-----------------------------------------------------------------------+-------------+-----------+
|                      | Description                                                           |   Type      | Default   |
+======================+=======================================================================+=============+===========+
| plot_int             | Frequency of plotfile output;                                         |    Int      | -1        |
|                      | if -1 then no plotfiles will be written at this frequency             |             |           |
+----------------------+-----------------------------------------------------------------------+-------------+-----------+
| plot_per_approx      | Time period of plotfile output (approximate); does not modify dt      |    Real     | -1        |
|                      | if -1 then no plotfiles will be written at this frequency             |             |           |
+----------------------+-----------------------------------------------------------------------+-------------+-----------+
| plotfile_on_restart  | Should we write a plotfile when we restart (only used if plot_int>0)  |   Bool      | False     |
+----------------------+-----------------------------------------------------------------------+-------------+-----------+
| plot_file            | Prefix to use for plotfile output                                     |  String     | plt       |
+----------------------+-----------------------------------------------------------------------+-------------+-----------+
| write_ls             | Should we write a plotfile holding the level set and volfrac?         |   Bool      | False     |
|                      | If true, it will only be written once,after initialization or restart |             |           |
+----------------------+-----------------------------------------------------------------------+-------------+-----------+
| write_eb_surface     | Should we write out the EB geometry in vtp format                     |   Bool      | False     |
|                      | If true, it will only be written once,after initialization or restart |             |           |
+----------------------+-----------------------------------------------------------------------+-------------+-----------+
| par_ascii_file       | Prefix to use for ascii particle output                               |  String     | par       |
+----------------------+-----------------------------------------------------------------------+-------------+-----------+
| par_ascii_int        | Frequency of ascii particle output;                                   |    Int      | -1        |
|                      | if -1 then no particle ascii files will be written                    |             |           |
+----------------------+-----------------------------------------------------------------------+-------------+-----------+
| par_ascii_per_approx | Time period of the ascii particle output (approximate);               |    Real     | -1        |
|                      | if -1 then particle ascii files will not be written at this frequency |             |           |
+----------------------+-----------------------------------------------------------------------+-------------+-----------+
| ascent_int           | Frequency of ascent pipeline;                                         |    Int      | -1        |
|                      | if -1 then ascent will not be called.                                 |             |           |
+----------------------+-----------------------------------------------------------------------+-------------+-----------+
| ascent_per_approx    | Time period of the ascent pipeline (approximate);                     |    Real     | -1        |
|                      | if -1 then ascent will not be called.                                 |             |           |
+----------------------+-----------------------------------------------------------------------+-------------+-----------+


The following inputs must be preceded by "amr" and control what variables will be written in plotfiles.

+---------------------+-----------------------------------------------------------------------+-------------+-----------+
|                     | Description                                                           |   Type      | Default   |
+=====================+=======================================================================+=============+===========+
| plt_regtest         | Save all variables to plot file (overrides all other IO flags)        |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_vel_g           | Save fluid velocity data to plot file                                 |    Int      | 1         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_ep_g            | Save fluid volume fraction to plot file                               |    Int      | 1         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_p_g             | Save fluid pressure to plot file                                      |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_ro_g            | Save fluid density to plot file                                       |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_mu_g            | Save fluid viscosity to plot file                                     |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_diveu           | Save div(ep_g . u) to plot file                                       |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_volfrac         | Save Eulerian grid volume fraction (from cut cells) to plot file      |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_gradp_g         | Save gradient of pressure filed to plot file                          |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_vort            | Save vorticity to plot file                                           |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_vel_p           | Save particle velocity to plot file                                   |    Int      | 1         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_radius          | Save particle radius to plot file                                     |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_volume          | Save particle volume to plot file                                     |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_volume          | Save particle volume to plot file                                     |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_mass            | Save particle mass to plot file                                       |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_ro_p            | Save particle density to plot file                                    |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_omoi            | Save (one divided by the) particle momentum of inertia to plot file   |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_mass            | Save particle mass to plot file                                       |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_omega_p         | Save particle angular velocity to plot file                           |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_drag_p          | Save particle drag force to plot file                                 |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| plt_phase           | Save particle type to plot file                                       |    Int      | 0         |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
+---------------------+-----------------------------------------------------------------------+-------------+-----------+


`Ascent <ascent.readthedocs.io>`_ has been integrated into MFIX-Exa for *in situ* visualization. 
For codes that have been built with Ascent support, the following inputs must be preceded by "ascent" 
and specifies the ascent actions for fluid and/or particles. The frequency which these are called 
is controlled by `ascent_int` or `ascent_per_approx`, see above. Note that if an ascent pipeline 
is being included in a GPU build/run, then you must enable managed memory, 
i.e., set `amrex.the_arena_is_managed = true`. 

+---------------------+-----------------------------------------------------------------------+-------------+-----------+
|                     | Description                                                           |   Type      | Default   |
+=====================+=======================================================================+=============+===========+
+---------------------+-----------------------------------------------------------------------+-------------+-----------+
| actions             | yaml file of the ascent actions (ex. ascent_actions.yaml). If no file |  String     |           |
|                     | name is provided, then calls to Ascent are skipped.                   |             |           |
+---------------------+-----------------------------------------------------------------------+-------------+-----------+




