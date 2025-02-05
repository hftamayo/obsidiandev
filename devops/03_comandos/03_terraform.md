

### Deploy basico de infrastructure:
- Escribir el tf file

- iniciar terraform:
` docker run --rm -v $(pwd):/workspace -w /workspace hashicorp/terraform init `

-  validar la configuracion:

` docker run --rm -v $(pwd):/workspace -w /workspace hashicorp/terraform validate `

- planning del deployment:

` docker run --rm -v $(pwd):/workspace -w /workspace hashicorp/terraform plan `

- apply the configuration:

` docker run --rm -v $(pwd):/workspace -w /workspace hashicorp/terraform apply `


- destruir todo lo creado:
` docker run --rm -v $(pwd):/workspace -w /workspace hashicorp/terraform destroy  `

- 
	- 