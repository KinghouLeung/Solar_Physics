# Solar Physics Data Pipeline
These Python scripts are written for three-channel solar observation data processing pipeline, including $\mathrm{H}\alpha$, $\mathrm{Ca}K$ and white light, at Shenzhen Astronomical Observatory. Based on the previous IDL version by Dr. Xiaofan Wang (wxf@nao.cas.cn), I updated the following aspects in my program.
1. Apart from filtering the candidate points of the limbs by gradient (`sobel`), I also used a cut-off level of the intensity of the image (after being smoothed by a `uniform filter`) to recognize the radial range of the limbs.
2. To reduce the influence of too-strong flares or limbs when finding the solar radius in the observed image, I applied `np.percentile(data, [1, 99])` to the original input data to clip the outliers.
3. I used `cv2.estimateAffinePartial2D(source_points, destination_points)` in `opencv` to figure out and align the features on the Sun between the observed data and GONG standard data, instead of applying the rotation simply by subjectively comparing. This alignment is intended to obtain the rotation matrix \mathbf{M}$ and should be repeated ~monthly.
\begin{equation*}
\mathbf{M} =
    \begin{bmatrix} \alpha & \beta & (1 - \alpha)x_{\mathrm{c}} - \beta y_{\mathrm{c}}\\
    -\beta & \alpha & \beta x_{\mathrm{c}} + (1 - \alpha)y_{\mathrm{c}}
\end{bmatrix}
\end{equation*}
4. In $\mathrm{H}\alpha$ images, I further added one step to strengthen the limbs feature linearly so that we can intuitively view the active edge regions without stretching the histogram.
