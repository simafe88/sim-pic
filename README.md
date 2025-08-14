# Install & load libraries
if (!require(keras)) install.packages("keras", dependencies = TRUE)
if (!require(tensorflow)) install.packages("tensorflow", dependencies = TRUE)
if (!require(magick)) install.packages("magick", dependencies = TRUE)

library(keras)
library(tensorflow)
library(magick)

# ---- LOAD TRAINED MODEL ----
# Replace 'model.h5' with your trained medical model file
model_path <- "model.h5"
if (!file.exists(model_path)) stop("Trained model file not found!")
model <- load_model_hdf5(model_path)

# ---- IMAGE PREPROCESSING ----
process_image <- function(image_path, target_size = c(224, 224)) {
  img <- image_read(image_path)
  img <- image_resize(img, paste0(target_size[1], "x", target_size[2], "!"))
  img_array <- as.numeric(as.array(img)) / 255
  img_array <- array_reshape(img_array, c(1, target_size[1], target_size[2], 3))
  return(img_array)
}

# ---- PREDICTION FUNCTION ----
diagnose_image <- function(image_path) {
  input_img <- process_image(image_path)
  prediction <- model %>% predict(input_img)
  
  if (prediction >= 0.5) {
    cat("✅ Likely positive for the disease (Probability:", round(prediction, 3), ")\n")
  } else {
    cat("❌ Likely negative for the disease (Probability:", round(prediction, 3), ")\
# sim-pic
just testing
