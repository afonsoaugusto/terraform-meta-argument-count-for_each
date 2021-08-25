# Trabalhando com os meta-argumentos do Terraform count e for_each com modulos

Essa semana na [mentoriaIaC](https://github.com/mentoriaiac/onboarding) iniciamos a implementação de uma issue que requeria a utilização de um modulo em loop.
Em questão é a [issue](https://github.com/mentoriaiac/iac-modulo-compute-cluster-gcp/issues/1) do modulo [iac-modulo-compute-cluster-gcp](https://github.com/mentoriaiac/iac-modulo-compute-cluster-gcp).
Em termos práticos é criar várias instancias no gcp para ser formar um cluster, e para isso é necessário que o modulo seja executado em loop.

No terraform nós temos dois recursos disponíveis para isso, a partir da versão 0.13.0 do terraform, o [count](https://www.terraform.io/docs/language/meta-arguments/count.html) e o [for_each](https://www.terraform.io/docs/language/meta-arguments/for_each.html).

## Possibilidades de implementação do modulo

Para termos alguns exemplos de como podemos implementar o modulo, vamos exemplificar qual o resultado que queremos e como podemos fazer.

Para não precisarmos de criar maquinas em uma cloud, podemos trabalhar com arquivos no filesystem que represetaram uma instancia.
Assim, todos vão ter as mesmas propriedades com valores diferentes.

```yaml
# servidor.yml
name: string
machine_type: string
boot_disk_size:
    initalize_params:
        image: string
network_interface:
    network: string
    subnetwork: string
startup_script: string
```

### Resultado esperado

O objetivo que queremos é que seja criado vários servidores com as mesmas propriedades, mas com valores diferentes.

```txt
├── objetivo
│   ├── params.yml
│   ├── control-plane-node-1.yml
│   ├── control-plane-node-2.yml
│   ├── control-plane-node-3.yml
│   ├── worker-node-1.yml
│   ├── worker-node-2.yml
│   └── worker-node-3.yml
```
