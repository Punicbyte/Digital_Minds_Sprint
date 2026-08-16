# Disentangling Persona and Emotion Representations: Digital Minds Research Sprint

This project was made for the Digital Minds Research Sprint.

The main question is whether persona representations and emotion representations in language models are actually separate, or whether they share some of the same internal representation.

I use Qwen2.5-7B-Instruct and focus on sycophancy as the persona trait. The project goes from extracting persona and emotion directions, to measuring their geometric overlap, to causal interventions, and finally to a controlled persona × valence experiment.

## What each stage does

### Stage 1 — Persona vector

`stage1_sycophancy.py`  
Extracts a sycophancy direction from contrastive sycophantic vs. independent system prompts and measures how stable that direction is across prompt pairs and layers.

`stage1_sycophancy_dataset.json`  
The system-prompt pairs and questions used to extract the persona representation.

`sycophancy_vectors.pt`  
Saved persona vectors and prompt-pair vectors.

`sycophancy_stability.csv`  
Measures how consistently the different prompt pairs recover the same persona direction across layers.

### Stage 2 — Emotion vectors

`stage2_emotion_vectors.py`  
Extracts emotion representations from implicit emotion stories, removes dominant neutral-text directions, and checks whether the representations are stable and recover valence/arousal structure.

`emotion_validation.csv`  
Split-half stability and valence/arousal validation results.

The large intermediate `emotion_vectors.pt` file is not included because it can be reproduced by running the Stage 2 code.

### Stage 3 — Geometry

`stage3_geometry.py`  
Measures how much the sycophancy representation overlaps the emotion subspace across layers and compares that overlap against a random-subspace baseline.

`stage3_layer_geometry.csv`  
Layer-level persona/emotion subspace overlap results.

`stage3_emotion_alignment.csv`  
Alignment between the persona vector and individual emotion directions.

### Stage 4 — Causal disentanglement

`stage4_causal.py`  
Decomposes an emotion direction into the part aligned with sycophancy and the orthogonal remainder, then tests how those interventions propagate through later layers.

`stage4_target_metadata.json`  
The selected layer/emotion and the geometry of the intervention directions.

`stage4_internal_propagation.csv`  
Average downstream movement along the persona and emotion coordinates.

`stage4_internal_per_question.csv`  
The same internal measurements broken down by example.

### Stage 5 — Persona × valence experiment

`stage5_factorial_dataset.jsonl`  
A controlled dataset crossing sycophantic vs. independent personas with positive vs. negative emotional stories across five persona-prompt pairs.

`stage5_extract_activations.py`  
Runs the factorial dataset through Qwen and saves story-token activations from every layer.

`stage5_factorial_analysis.py`  
Tests whether emotion directions survive persona changes, whether persona directions survive valence changes, and how large the persona × valence interaction is.

`stage5_factorial_layer_results.csv`  
The main Stage 5 results across layers.

`stage5_factorial_pair_results.csv`  
The same analysis separately for each persona-prompt pair.

The large Stage 5 activation tensor is not included because it can be regenerated from the dataset and extraction code.

## Main takeaway

Persona and emotion representations overlap geometrically and can causally interact, but the controlled factorial experiment suggests that the broader representations are still highly stable across changes in the other factor. In other words, overlap does not necessarily mean the two representations are inseparable.

This is evidence about model representations and behavior, not evidence that the model subjectively experiences emotions.
