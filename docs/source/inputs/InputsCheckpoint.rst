.. _Chap:InputsCheckpoint:

Checkpoint/Restart
==================

The following inputs must be preceded by "amr" and control checkpoint/restart.

+-------------------------+-----------------------------------------------------------------------+-------------+-----------+
|                         | Description                                                           |   Type      | Default   |
+=========================+=======================================================================+=============+===========+
| restart                 | If present, then the name of file to restart from                     |    String   | None      |
+-------------------------+-----------------------------------------------------------------------+-------------+-----------+
| check_int               | Frequency of checkpoint output;                                       |    Int      | -1        |
|                         | if -1 then no checkpoints will be written                             |             |           |
+-------------------------+-----------------------------------------------------------------------+-------------+-----------+
| check_file              | Prefix to use for checkpoint output                                   |  String     | chk       |
+-------------------------+-----------------------------------------------------------------------+-------------+-----------+
| check_walltime          | Write a check point file after the specified walltime (HH:MM:SS)      |  String     | None      |
|                         | has lapsed. For example, if amr.check_waltime = 00:10:00, then a      |             |           |
|                         | checkpoint file is after a job has run for ten minutes.               |             |           |
+-------------------------+-----------------------------------------------------------------------+-------------+-----------+
| write_geom_chk          | When True, writes the EB geometry data into a checkpoint file.        |  bool       | False     |
+-------------------------+-----------------------------------------------------------------------+-------------+-----------+
| use_geom_chk_if_present | When True, looks for the EB geometry checkpoint file in the           |  bool       | True      |
|                         | current directory and uses it, overriding any EB geometry in inputs   |             |           |
+-------------------------+-----------------------------------------------------------------------+-------------+-----------+
