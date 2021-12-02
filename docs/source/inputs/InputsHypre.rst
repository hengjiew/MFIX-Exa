.. _Chap:InputsHypre:

Hypre Inputs
=============

The following inputs control the hypre settings and are read directly 
by AMReX when we use hypre as the bottom solver.

NOTE: Default uses the same hypre namespace "hypre" for all solvers. However, 
the hypre namespace can be set at the solver level and the hypre settings 
can be configured for each MLMG solver.

These settings must be preceded by the hypre_namespace setting corresponding to the solver (see :ref:`Chap:InputsMultigrid`)

+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
|                                   |  Description                                                          |   Type      | Default      |
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| verbose                           |  Verbosity of hypre                                                   |   Int       |   0          |
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| adjust_singular_matrix            |  Should be true if the problem to be solved has singular matrix       |   Bool      |   false      | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| hypre_solver                      |  Type of hypre solver                                                 |   String    |  BoomerAMG   |
|                                   |                                                                       |             |              |
|                                   |  Options are BoomerAMG, GMRES, COGMRES, LGMRES, FlexGMRES, BiCGSTAB,  |             |              |
|                                   |  PCG or Hybrid                                                        |             |              |
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| hypre_preconditioner              |  Type of preconditioner                                               |   String    |   none       |
|                                   |                                                                       |             |              |
|                                   |  Options are BoomerAMG or euclid                                      |             |              |
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| recompute_preconditioner          |  Recompute preconditioner during runs                                 |    Bool     |   true       | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| write_matrix_files                |  Write out matrix into text files                                     |    Bool     |   false      | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| overwrite_existing_matrix_files   |  Over-write existing matrix files                                     |    Bool     |   false      | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| bamg_verbose                      |  Verbosity of BoomerAMG preconditioner                                |    Int      |   0          | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| bamg_logging                      |  When using BoomerAMG preconditioner                                  |    Int      |   0          | 
|                                   |                                                                       |             |              | 
|                                   |  See HYPRE_BoomerAMGSetLogging                                        |             |              | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| bamg_coarsen_type                 |  When using BoomerAMG preconditioner                                  |    Int      |   6          | 
|                                   |                                                                       |             |              | 
|                                   |  See HYPRE_BoomerAMGSetCoarsenType                                    |             |              | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| bamg_cycle_type                   |  When using BoomerAMG preconditioner                                  |    Int      |   1          | 
|                                   |                                                                       |             |              | 
|                                   |  See HYPRE_BoomerAMGSetCycleType                                      |             |              | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| bamg_relax_type                   |  When using BoomerAMG preconditioner                                  |    Int      |   6          | 
|                                   |                                                                       |             |              | 
|                                   |  See HYPRE_BoomerAMGSetRelaxType                                      |             |              | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| bamg_relax_order                  |  When using BoomerAMG preconditioner                                  |    Int      |   1          | 
|                                   |                                                                       |             |              | 
|                                   |  See HYPRE_BoomerAMGSetRelaxOrder                                     |             |              | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| bamg_num_sweeps                   |  When using BoomerAMG preconditioner                                  |    Int      |   2          | 
|                                   |                                                                       |             |              | 
|                                   |  See HYPRE_BoomerAMGSetNumSweeps                                      |             |              | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| bamg_max_levels                   |  When using BoomerAMG preconditioner                                  |    Int      |   20         | 
|                                   |                                                                       |             |              | 
|                                   |  See HYPRE_BoomerAMGSetMaxLevels                                      |             |              | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| bamg_strong_threshold             |  When using BoomerAMG preconditioner                                  |    Real     |   0.57       | 
|                                   |                                                                       |             |              | 
|                                   |  See HYPRE_BoomerAMGSetStrongThreshold                                |             |              | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
| bamg_interp_type                  |  When using BoomerAMG preconditioner                                  |    Int      |   0          | 
|                                   |                                                                       |             |              | 
|                                   |  HYPRE_BoomerAMGSetInterpType                                         |             |              | 
+-----------------------------------+-----------------------------------------------------------------------+-------------+--------------+
