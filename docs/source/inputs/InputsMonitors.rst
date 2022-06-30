.. _Chap:InputsMonitors:

Monitors
========

The following inputs must be preceded by "amr" and control whether to compute
monitors, i.e., spatial averages, and how often to output the results.
n is the number of monitors implicitly defined by the size of avg_region_x_w.

+------------------+-----------------------------------------------------------------------+-------------+-----------+
|                  | Description                                                           |   Type      | Default   |
+==================+=======================================================================+=============+===========+
| avg_int          | Interval, in number of CFD dt's, to write output                      |  Int        | -1        |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_file         | Base file name which is appended with the data type (vel_p, p_g,      |  String     | avg_region|
|                  | ep_g or vel_g), the number of this type of averaging,  and the .csv   |             |           |
|                  | file extension                                                        |             |           |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_ep_g         | Average and save fluid-phase volume fraction (if 1)                   |  n*Int      | 0         |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_p_g          | Average and save fluid-phase pressure (if 1)                          |  n*Int      | 0         |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_vel_g        | Average and save fluid-phase velocity (if 1)                          |  n*Int      | 0         |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_T_g          | Average and save fluid-phase temperature (if 1)                       |  n*Int      | 0         |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_ro_p         | Average and save particle density (if 1)                              |  n*Int      | 0         |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_vel_p        | Average and save particle velocity (if 1)                             |  n*Int      | 0         |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_T_p          | Average and save particle temperature (if 1)                          |  n*Int      | 0         |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_region_x_w   | Lower bound of averaging region in x-direction                        |  n*Real     | None      |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_region_x_e   | Upper bound of averaging region in x-direction                        |  n*Real     | None      |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_region_y_s   | Lower bound of averaging region in y-direction                        |  n*Real     | None      |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_region_y_n   | Upper bound of averaging region in y-direction                        |  n*Real     | None      |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_region_z_b   | Lower bound of averaging region in z-direction                        |  n*Real     | None      |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| avg_region_z_t   | Upper bound of averaging region in z-direction                        |  n*Real     | None      |
+------------------+-----------------------------------------------------------------------+-------------+-----------+

A Monitor is a tool for capturing data from the solver about the model.

Data (such as volume fraction, pressure, velocity, etc.) for a given
:ref:`monitor_region` is written to a :ref:`CSV file <monitor_filename>`
while the solver is running.


Region Selection
----------------

To define a monitor, there must be a region already defined in the regions
inputs. A Monitor region is a single point, plane, or volume. Multiple regions
cannot be combined for a monitor.

+------------------+-----------------------------------------------------------------------+-------------+-----------+
|                  | Description                                                           |   Type      | Default   |
+==================+=======================================================================+=============+===========+
| region           | Define the region in which the monitor will be computed               |  String     | None      |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| plane            | For LagrangianMonitors::FlowRate, defines the plane through which     |  String     | None      |
|                  | the flow rate of the particles in the [region] will be computed       |             |           |
+------------------+-----------------------------------------------------------------------+-------------+-----------+


Monitor Output
--------------

The monitor data will be output to the Filename base with the extension
``.csv``. The monitor output file is in Comma Separated Value (CSV) format. The
first line of the file provides header information.

+------------------+-----------------------------------------------------------------------+-------------+-----------+
|                  | Description                                                           |   Type      | Default   |
+==================+=======================================================================+=============+===========+
| plot_file        | Define the name of the plotfile where monitor output will be saved    |  String     | None      |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| plot_int         | Define the timestep frequency for saving monitored data to file       |  Int        | -1        |
+------------------+-----------------------------------------------------------------------+-------------+-----------+
| plot_per_approx  | Define the approximated simulation time at which saving monitored     |  Real       | 0         |
|                  | data                                                                  |             |           |
+------------------+-----------------------------------------------------------------------+-------------+-----------+


Monitor Variables
-----------------

+------------------+-----------------------------------------------------------------------+-------------+-----------+
|                  | Description                                                           |   Type      | Default   |
+==================+=======================================================================+=============+===========+
| variables        | Define which variables are to be monitored by this monitor            |  String     | None      |
+------------------+-----------------------------------------------------------------------+-------------+-----------+


Eulerian Monitors
-----------------

There are different types of monitors available. A monitor type applies an
operator (for example a sum, an area integral or a volume integral) to the
variable. The dimensionality of the region determines which operators can be
applied.


The table below summarizes the nomenclature used to describe the monitor
operators:

========================= =========================================
Symbol                     Description
========================= =========================================
:math:`\phi_{ijk}`        Variable value at indexed cell
:math:`\varepsilon_{ijk}` Phase **volume fraction** at indexed cell
:math:`\rho_{ijk}`        Phase **density** at indexed cell
:math:`\vec{v}_{ijk}`     Phase **velocity** at indexed cell
:math:`A_{ijk}`           Cross-sectional area of cell
:math:`V_{ijk}`           Volume of indexed cell
========================= =========================================


Point Region
~~~~~~~~~~~~

For a point region, the monitor data value is simply the value of the variable
at that point:

Value
   Returns the value of the field quantity in the selected region.

   .. math:: \phi_{ijk}


Area or Volume Region
~~~~~~~~~~~~~~~~~~~~~

The following  monitor types are valid for area and volume regions:

Sum
   The sum is computed by summing all values of the field quantity in the
   selected region.

   .. math:: \sum_{ijk}\phi_{ijk}

Min
   Minimum value of the field quantity in the selected region.

   .. math:: \min_{ijk} \phi_{ijk}

Max
   Maximum value of the field quantity in the selected region.

   .. math:: \max_{ijk} \phi_{ijk}

Average
   Average value of the field quantity in the selected region where :math:`N` is
   the total number of observations (cells) in the selected region.

   .. math:: \phi_0 = \frac{\sum_{ijk} \phi_{ijk}}{N}

Standard Deviation
   The standard deviation of the field quantity in the selected region where
   :math:`\phi_0` is the average of the variable in the selected region.

   .. math:: \sigma_{\phi} = \sqrt{\frac{ \sum_{ijk} (\phi_{ijk}-\phi_{0})^2 }{N}}


Surface Integrals
~~~~~~~~~~~~~~~~~

The following types are only valid for area regions:

Area
   Area of selected region is computed by summing the areas of the facets that
   define the surface.

   .. math:: \int dA = \sum_{ijk} \lvert A_{ijk} \rvert

Area-Weighted Average
   The area-weighted average is computed by dividing the summation of the
   product of the selected variable and facet area by the total area of the
   region.

   .. math:: \frac{\int\phi dA}{A} = \frac{\sum_{ijk}{\phi_{ijk} \lvert A_{ijk} \rvert}}{\sum_{ijk}{\lvert A_{ijk} \rvert}}

Flow Rate
   The flow rate of a field variable through a surface is computed by summing
   the product of the phase volume fraction, density, the selected field
   variable, phase velocity normal to the facet :math:`v_n`, and the facet area.

   .. math:: \int\varepsilon\rho\phi{v_n}dA = \sum_{ijk}\varepsilon_{ijk}\rho_{ijk}\phi_{ijk} {v}_{n,ijk} \lvert A_{ijk} \rvert

Mass Flow Rate
   The mass flow rate through a surface is computed by summing the product of
   the phase volume fraction, density, phase velocity normal to the facet
   :math:`v_n`, and the facet area.

   .. math:: \int\varepsilon\rho{v_n} dA = \sum_{ijk}\varepsilon_{ijk}\rho_{ijk}{v}_{n,ijk}  \lvert A_{ijk} \rvert

Mass-Weighted Average
   The mass flow rate through a surface is computed by summing the product of
   the phase volume fraction, density, phase velocity normal to the facet, and
   the facet area.

   .. math:: \frac{\int\varepsilon\rho\phi{v_n}dA}{\int\varepsilon\rho{v_n}dA} = \frac{\sum_{ijk}\varepsilon_{ijk}\rho_{ijk}\phi_{ijk} {v}_{n,ijk} \lvert A_{ijk} \rvert}{\sum_{ijk}\varepsilon_{ijk}\rho_{ijk} {v}_{n,ijk} \lvert A_{ijk} \rvert}

Volume Flow Rate
   The volume flow rate through a surface is computed by summing the product of
   the phase volume fraction, phase velocity normal to the facet :math:`v_n`,
   and the facet area.

   .. math:: \int\varepsilon{v_n}dA = \sum_{ijk}\varepsilon_{ijk}{v}_{n,ijk} \lvert A_{ijk} \rvert


Volume Integrals
~~~~~~~~~~~~~~~~

The following types are only valid for volume regions:

Volume
   The volume is computed by summing all of the cell volumes in the selected
   region.

   .. math:: \int  dV = \sum_{ijk}{ \lvert V_{ijk}} \rvert

Volume Integral
   The volume integral is computed by summing the product of the selected field
   variable and the cell volume.

   .. math:: \int \phi dV = \sum_{ijk}{\phi_{ijk} \lvert V_{ijk}} \rvert

Volume-Weighted Average
   The volume-weighted average is computed by dividing the summation of the
   product of the selected field variable and cell volume by the sum of the cell
   volumes.

    .. math:: \frac{\int\phi dV}{V} = \frac{\sum_{ijk}{\phi_{ijk} \lvert V_{ijk} \rvert}}{\sum_{ijk}{\lvert V_{ijk} \rvert}}

Mass-Weighted Integral
   The mass-weighted integral is computed by summing the product of phase volume
   fraction, density, selected field variable, and cell volume.

   .. math:: \int \varepsilon\rho\phi dV = \sum_{ijk}\varepsilon_{ijk}\rho_{ijk}\phi_{ijk} \lvert V_{ijk}\rvert

Mass-Weighted Average
   The mass-weighted average is computed by dividing the sum of the product of
   phase volume fraction, density, selected field variable, and cell volume by
   the summation of the product of the phase volume fraction, density, and cell
   volume.

   .. math:: \frac{\int\phi\rho\varepsilon dV}{\int\rho\varepsilon dV} = \frac{\sum_{ijk}\varepsilon_{ijk}\rho_{ijk}\phi_{ijk} \lvert V_{ijk}\rvert}{\sum_{ijk}\varepsilon_{ijk}\rho_{ijk} \lvert V_{ijk}\rvert}



Lagrangian Monitors
-------------------

There are different types of monitors available. A monitor type applies an
operator (for example a sum, an area integral or a volume integral) to the
variable. The dimensionality of the region determines which operators can be
applied.


The table below summarizes the nomenclature used to describe the monitor
operators:

========================= ====================================================
Symbol                     Description
========================= ====================================================
:math:`\phi_p`            Variable value of the indexed particle
:math:`m_p`               **Mass** of the indexed particle
:math:`V_p`               **Volume** of the indexed particle
:math:`\mathcal{w}_p`     **Statistical weight** of the indexed particle [#]_
========================= ====================================================

.. [#] *The statistical weight is one for DEM simulations.*


General particle properties
~~~~~~~~~~~~~~~~~~~~~~~~~~~

General particle properties can be obtained from area (plane) and volume
regions. For area regions, all particles in Eulerian cells that intersect the
plane are used in evaluating the average.


Sum
   The sum of particle property, :math:`\phi_p` in the selected region is
   calculated using the following expression.

   .. math:: \sum_p w_p \phi_p

Min
   The minimum value of particle property :math:`phi_p` is the selected region
   is obtained using the following expression.

   .. math:: \min_p \phi_p

Max
   The maximum value of particle property :math:`phi_p` is the selected region
   is obtained using the following expression.

   .. math:: \max_p \phi_p


Averaged particle properties
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Particle properties can be averaged over area (plane) and volume regions. For
area regions, all particles in Eulerian cells that intersect the plane are used
in evaluating the average.


Average
   The average value of particle property, :math:`\phi_p` in the selected region
   is calculated using the following expression. For DEM simulations, the
   statistical weight of a particle, :math:`w_p`, is one such that the sum of
   the weights is the total number of observations in the selected region.

   .. math:: \bar{\phi} = \frac{\sum_p w_p \phi_p}{\sum_p w_p}

Standard Deviation
   The standard deviation of particle property, :math:`phi_p` in the selected
   region is calculated using the following expression.  :math:`\bar{\phi}` is
   the averaged variable in the selected region.

   .. math:: \sigma_{\phi} = \sqrt{\frac{ \sum_p w_p (\phi_p-\bar{\phi})^2 }{\sum_p w_p}}

Mass-weighted average
   Mass-weighted average value of particle property, :math:`\phi_p` in the
   selected region is calculated using the following expression.

   .. math:: \bar{\phi}_m = \frac{\sum_{p} w_p m_p \phi_p}{\sum_p w_p m_p }

Volume-weighted average
   Volume-weighted average value of particle property, :math:`\phi_p` in the
   selected region is calculated using the following expression.

   .. math:: \bar{\phi}_v = \frac{\sum_{p} w_p V_p \phi_p}{\sum_p w_p V_p}


Flow rates
~~~~~~~~~~

Flow rate monitors for Lagrangian particles (DEM/PIC) are only valid for area
(plane) regions. The set of particles crossing the flow plane, :math:`\Gamma` is
approximated using the height of the plane, :math:`h`, the position of the
particle, :math:`x_p`, and the particle velocity normal to the plane,
:math:`v_p` such that

   .. math:: (v_p)(\frac{x_p - h}{\Delta t}) > 0

and

   .. math:: \left|v_p\right| \geq  \left| \frac{x_p - h}{\Delta t} \right|


Flow rate
   The net flow rate of a general particle property :math:`\phi_p` is computed
   by summing the properties of the set of particles projected to have crossed
   the flow plane, :math:`\Gamma`.

   .. math:: \sum_{p \in \Gamma} w_p \phi_p \frac{v_p}{\left| v_p \right|}

Mass-weighted flow rate
   The net mass-weighted flow rate is the sum of the general particle property
   :math:`\phi_p` multiplied by the particle mass, :math:`m_p` of the set of
   particles projected to have crossed the flow plane, :math:`\Gamma`.

   .. math:: \sum_{p \in \Gamma} w_p m_p \phi_p \frac{v_p}{\left| v_p \right|}

Volume-weighted flow rate
   The net volume-weighted flow rate is the sum of the general particle property
   :math:`\phi_p` multiplied by the particle volume, :math:`V_p` of the set of
   particles projected to have crossed the flow plane, :math:`\Gamma`.

   .. math:: \sum_{p \in \Gamma}\phi_p w_p V_p \frac{v_p}{\left| v_p \right|}
