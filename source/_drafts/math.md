##
$$
\overrightarrow{F} = m \overrightarrow{a}
$$ 

<div align="center"><img style="background: white;" src="svg\gFSR60aV52.svg"></div>

$$
\overrightarrow{F} = {\rho} \overrightarrow{a}
$$

<div align="center"><img style="background: white;" src="svg\RrXOkeICCu.svg"></div> 

$$
\overrightarrow{F} = {\overrightarrow{F}}^{External} + {\overrightarrow{F}}^{Pressure} + {\overrightarrow{F}}^{Viscosity}
$$

<div align="center"><img style="background: white;" src="svg\XRvvoNDw7I.svg"></div>


$$
{\overrightarrow{F}}^{External} = {\rho} \overrightarrow{g}
$$

<div align="center"><img style="background: white;" src="svg\FIvr4TkIaq.svg"></div>

$$
{\overrightarrow{F}}^{Pressure} = -\nabla p
$$

<div align="center"><img style="background: white;" src="svg\1MNPHX1r8o.svg"></div>

$$
{\overrightarrow{F}}^{Viscosity} = \mu {\nabla^2} \overrightarrow{u}
$$

<div align="center"><img style="background: white;" src="svg\ObLgP4IQiY.svg"></div>

$$
{\rho} \overrightarrow{a} = {\rho} \overrightarrow{g} - \nabla p + \mu {\nabla^2} \overrightarrow{u}
$$

<div align="center"><img style="background: white;" src="svg\j8d1h7Ujgx.svg"></div>

$$
\overrightarrow{a} = \overrightarrow{g} - \cfrac{\nabla p}{{\rho}} + \cfrac{\mu {\nabla^2} \overrightarrow{u}}{{\rho}}
$$

<div align="center"><img style="background: white;" src="svg\YtSXdV9crH.svg"></div>

$$
\overrightarrow{a}(r_i) = \overrightarrow{g} - \cfrac{\nabla p(r_i)}{{\rho(r_i)}} + \cfrac{\mu {\nabla^2} \overrightarrow{u}(r_i)}{{\rho(r_i)}}
$$

<div align="center"><img style="background: white;" src="svg\nY6inmMCzk.svg"></div>

$$
A(\overrightarrow{r}) = \sum_{j}^{} A_j \cfrac{m_j}{\rho_j} W(\overrightarrow{r}-\overrightarrow{r_j},h)
$$

<div align="center"><img style="background: white;" src="svg\5CPHL7ldop.svg"></div>

$$
\rho(r_i) = \sum_{j}^{} \rho_j \cfrac{m_j}{\rho_j} W(\overrightarrow{r_i}-\overrightarrow{r_j},h)
$$

<div align="center"><img style="background: white;" src="svg\8EbGqKCRe9.svg"></div>

$$
\rho(r_i) = \sum_{j}^{} m_j W(\overrightarrow{r_i}-\overrightarrow{r_j},h)
$$

<div align="center"><img style="background: white;" src="svg\Sv2emhmTjc.svg"></div>

$$
W_{Poly6}(\overrightarrow{r},h)= \begin{cases} K_{Poly6}(h^2-r^2)^3&0\leqslant r\leqslant h\\ 0&otherwise \end{cases}
$$

<div align="center"><img style="background: white;" src="svg\hz7Ss4oa1R.svg"></div>

$$
K_{Poly6} = \cfrac{1}{{\int_{0}^{2\pi}}{\int_{0}^{\pi}}{\int_{0}^{h}}r^2\sin(\varphi)(h^2-r^2)^3dr d\varphi d\theta} = \cfrac{315}{64\pi h^9}
$$

<div align="center"><img style="background: white;" src="svg\OYZjIckEMK.svg"></div>

$$
\rho(r_i) = \sum_{j}^{} m_j W(\overrightarrow{r_i}-\overrightarrow{r_j},h) = \sum_{j}^{} m \cfrac{315}{64\pi h^9} (h^2-|\overrightarrow{r_i}-\overrightarrow{r_j}|^2)^3
$$

<div align="center"><img style="background: white;" src="svg\pcGjlDVZOg.svg"></div>

$$
\rho(r_i) =  m \cfrac{315}{64\pi h^9} \sum_{j}^{}(h^2-|\overrightarrow{r_i}-\overrightarrow{r_j}|^2)^3
$$

<div align="center"><img style="background: white;" src="svg\X7UO8Lxr7y.svg"></div>

$$
-\nabla p(\overrightarrow{r_i}) = - \sum_{j}^{} p_j \cfrac{m_j}{\rho_j} \nabla W(\overrightarrow{r_i}-\overrightarrow{r_j},h)
$$

<div align="center"><img style="background: white;" src="svg\k2DmuNR60U.svg"></div>

$$
-\nabla p(\overrightarrow{r_i}) = - \sum_{j}^{}  \cfrac{m_j(p_i+p_j)}{2\rho_j} \nabla W(\overrightarrow{r_i}-\overrightarrow{r_j},h)
$$

<div align="center"><img style="background: white;" src="svg\bWOTrqJ4ZE.svg"></div>

$$
p = K(\rho-\rho_0)
$$

<div align="center"><img style="background: white;" src="svg\lN07o0gO5k.svg"></div>

$$
W_{Spiky}(\overrightarrow{r},h)= \begin{cases} K_{Spiky}(h-r)^3&0\leqslant r\leqslant h\\ 0&otherwise \end{cases}
$$

<div align="center"><img style="background: white;" src="svg\JztPC3kwFT.svg"></div>

$$
K_{Spiky} = \cfrac{15}{\pi h^6}
$$

<div align="center"><img style="background: white;" src="svg\7ZyREk1GQO.svg"></div>

$$
-\nabla p(\overrightarrow{r_i}) = - \sum_{j}^{}  \cfrac{m_j(p_i+p_j)}{2\rho_j} \nabla \cfrac{15}{\pi h^6}(h-|\overrightarrow{r_i}-\overrightarrow{r_j}|)^3
$$

<div align="center"><img style="background: white;" src="svg\ZDtCdL2u2v.svg"></div>

$$
\nabla A = \overrightarrow{i} \cfrac{\partial f}{\partial x} + \overrightarrow{j} \cfrac{\partial f}{\partial y} + \overrightarrow{k} \cfrac{\partial f}{\partial z}
$$

<div align="center"><img style="background: white;" src="svg\b6T6cU2eCg.svg"></div>

$$
A = f(x,y,z)
$$ 

<div align="center"><img style="background: white;" src="svg\xeqov2GTei.svg"></div>

$$
-\nabla p(\overrightarrow{r_i}) = - \sum_{j}^{}  \cfrac{m_j(p_i+p_j)}{2\rho_j}  \cfrac{15}{\pi h^6}(-3)\cfrac{\overrightarrow{r_i}-\overrightarrow{r_j}}{|\overrightarrow{r_i}-\overrightarrow{r_j}|}(h-|\overrightarrow{r_i}-\overrightarrow{r_j}|)^2
$$

<div align="center"><img style="background: white;" src="svg\zx0v22k9Ty.svg"></div>

$$
-\nabla p(\overrightarrow{r_i}) = m\cfrac{45}{\pi h^6} \sum_{j}^{}  \cfrac{(p_i+p_j)}{2\rho_j}  \cfrac{\overrightarrow{r_i}-\overrightarrow{r_j}}{|\overrightarrow{r_i}-\overrightarrow{r_j}|}(h-|\overrightarrow{r_i}-\overrightarrow{r_j}|)^2
$$

<div align="center"><img style="background: white;" src="svg\yRDfhJ98AK.svg"></div>

$$
\mu {\nabla^2} \overrightarrow{u}(r_i) = \mu \sum_{j}^{} \mu_j \cfrac{m_j}{\rho_j}\nabla^2W(\overrightarrow{r_i}-\overrightarrow{r_j},h)
$$

<div align="center"><img style="background: white;" src="svg\JKdAmLEBl1.svg"></div>

$$
\mu {\nabla^2} \overrightarrow{u}(r_i) = \mu \sum_{j}^{} m_j \cfrac{\overrightarrow{u_j}-\overrightarrow{u_i}}{\rho_j}\nabla^2W(\overrightarrow{r_i}-\overrightarrow{r_j},h)
$$

<div align="center"><img style="background: white;" src="svg\i2yBvKDkPX.svg"></div>

$$
W_{viscosity}(\overrightarrow{r},h)= \begin{cases} K_{viscosity}(-\cfrac{r^3}{2h^3}+\cfrac{r^2}{h^2}+\cfrac{h}{2r}-1)&0\leqslant r\leqslant h\\ 0&otherwise \end{cases}
$$

<div align="center"><img style="background: white;" src="svg\VaVqSUDL2Y.svg"></div>

$$
with \ r=|\overrightarrow{r}|
$$

<div align="center"><img style="background: white;" src="svg\t3QUBSMg5R.svg"></div>

$$
K_{viscosity} = \cfrac{15}{2\pi h^3}
$$

<div align="center"><img style="background: white;" src="svg\acg3FrpLaJ.svg"></div>

$$
\nabla^2W(\overrightarrow{r},h)= \nabla ^2 \cfrac{15}{2\pi h^3} (-\cfrac{r^3}{2h^3}+\cfrac{r^2}{h^2}+\cfrac{h}{2r}-1)
$$

<div align="center"><img style="background: white;" src="svg\XUrZK2iH7N.svg"></div>

$$
\nabla^2 A = \cfrac{\partial^2 A}{\partial x^2} + \cfrac{\partial^2 A}{\partial y^2} + \cfrac{\partial^2 A}{\partial z^2}
$$

<div align="center"><img style="background: white;" src="svg\yZnrcY2diW.svg"></div>

$$
\nabla^2W(\overrightarrow{r},h)= \cfrac{15}{2\pi h^3} (-\cfrac{1}{2h^3}(6r)+\cfrac{2}{h^2} +\cfrac{h}{2}(\cfrac{2}{h^3} )  )=\cfrac{45}{2\pi h^6} (h-r)
$$

<div align="center"><img style="background: white;" src="svg\gk9byA8dQH.svg"></div>

$$
\mu {\nabla^2} \overrightarrow{u}(r_i) = \mu \sum_{j}^{} m_j \cfrac{\overrightarrow{u_j}-\overrightarrow{u_i}}{\rho_j} \cfrac{45}{2\pi h^6} (h-|\overrightarrow{r_i}-\overrightarrow{r_j}|) = \mu m \cfrac{45}{2\pi h^6}\sum_{j}^{}  \cfrac{\overrightarrow{u_j}-\overrightarrow{u_i}}{\rho_j}  (h-|\overrightarrow{r_i}-\overrightarrow{r_j}|)
$$

<div align="center"><img style="background: white;" src="svg\o9A33Cvjcg.svg"></div>

$$
\mu {\nabla^2} \overrightarrow{u}(r_i) = \mu m \cfrac{45}{2\pi h^6}\sum_{j}^{}  \cfrac{\overrightarrow{u_j}-\overrightarrow{u_i}}{\rho_j}  (h-|\overrightarrow{r_i}-\overrightarrow{r_j}|)
$$

<div align="center"><img style="background: white;" src="svg\li2uNAX7O5.svg"></div>

$$
p_i = \cfrac{EosScale}{EosExponent}((\cfrac{\rho(\overrightarrow{r_i})}{\rho_0})^{EosExponent}-1)
$$

<div align="center"><img style="background: white;" src="svg\ni4vNQMyz7.svg"></div>






























