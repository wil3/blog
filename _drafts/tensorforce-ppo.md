optimization_steps - optim_epochs?
batch_size - optim_batchsize
frequency - 

entropy_regularization

Update model spec is used here: models/memory_model.py
"update_mode": {

What are the other options?  Timesteps, sequences 
"unit": "episodes", 

"batch_size": 10,

if frequency condition is hit - pulls a batch from the memory and does one
optimization step. This is the horizona (T)
"frequency": 10
},



"memory": {
"type": "latest",
"include_next_states": false,
"capacity": 5000
},

"step_optimizer": {
"type": "adam",
"learning_rate": 1e-3
},
"subsampling_fraction": 0.1,
"optimization_steps": 50,

"discount": 0.99,
"entropy_regularization": 0.01,
"gae_lambda": null,
"likelihood_ratio_clipping": 0.2,

"baseline_mode": "states",
"baseline": {
"type": "mlp",
"sizes": [32, 32]
},
"baseline_optimizer": {
"type": "multi_step",
"optimizer": {
	"type": "adam",
	"learning_rate": 1e-3
},
"num_steps": 5
},

