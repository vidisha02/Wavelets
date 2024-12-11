The application of image segmentation is shown in a variety of areas: medical imaging,
remote sensing, and autonomous driving. Conventional architectures in the design of U-Nets rely on max-pooling or average pooling techniques to reduce features. Sometimes, the techniques lead to loss of spatial information that is essential for precise segmentation. In
the present work, we introduced a new form of the double skip U-Net architecture with the
DWT as a pooling technique that encourages a better preservation of their multi-resolution
characteristics. Using wavelet-based pooling, our objective is to conserve the essential
spatial and frequency details achieved during the down-sampling process in order to obtain
better accuracy for segmentation. We also apply IDWT for up-sampling thus ensuring
accurate reconstruction at the decoding stage. The findings from our experiments indicate
that the wavelet-enhanced double skip U-Net exhibits superior performance compared to
conventional U-Net models in segmentation tasks, especially in contexts that necessitate
meticulous detail retention and the capture of multi-scale features. This methodology
holds potential for applications demanding high-fidelity segmentation and may be further
adapted to enhance other deep learning models in subsequent research.

Model Architecture:

Encoder:For the encoder path, DWT is used at every down-sampling for each feature
map. DWT decomposes the feature map into four different components:
1. LL: The approximation sub-band that reflects low-frequency content with more or
less preservation of structure of the feature map.
2. HH,HL,LH: These are high-frequency sub-bands that contain edge detail, textures,
etc., and other high-resolution structures within an image.

Double skip connections: To ensure the effective transfer of these frequency components, the model incorporates double skip connections at each down-sampling level:
1. Up skip: The low-frequency component (LL) is transferred directly from the encoder
to the corresponding layer in the decoder via an up skip connection. This helps
retain the coarse structure and spatial context in the decoder.
2. Down skip: The high frequency components (LH, HL, HH) are passed through the
decoder with a down skip connection, retaining all the important fine details that
ensure proper boundaries of segmentation.

Decoder: The process in the decoder is as follows:
1. The IDWT of the new low-frequency component (LL) and the high-frequency components (LH, HL, HH) from the down skip connection is taken, effectively reconstructing a more detailed and accurate up-sampled feature map.
2. The output of the IDWT is then concatenated with the low-frequency component
(LL) transferred via the up skip connection, providing the decoder with both high-level context and spatial detail.
3. This concatenated feature map is subsequently passed through a series of convolution layers to refine the feature representation and extract segmentation-specific
information at each layer of up-sampling.

The encoder consists of four down-sampling blocks. Each block begins with a convolutional layer, followed by batch normalization and Leaky ReLU activation. This sequence
is repeated twice within each down-sampling block, ensuring that feature extraction is
both refined and consistent across layers.
6
The decoder includes four up-sampling blocks. Each block begins with an inverse discrete
wavelet transform (IDWT) to up-sample the feature maps. The output of the IDWT is
then concatenated with the corresponding features from the up skip connections, after
which the block applies a convolutional layer, batch normalization, and Leaky ReLU activation. This structure aids in reconstructing the segmentation map while preserving
high-resolution spatial information from the encoder.
