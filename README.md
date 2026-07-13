# TechMarket · SharedWorkflows para K3S

**Evaluación Final Transversal · AUY1104**
**Catalina Peña Mora**


Workflows reutilizables de GitHub Actions, pensados para ser consumidos por otros repos vía `workflow_call` en vez de duplicar lógica de CI/CD en cada proyecto.

## `deploy-api.yaml`

Receta compartida que implementa:

- Build de la imagen Docker y publicación en Docker Hub
- Despliegue **Blue-Green** sobre un clúster K3s, manipulando directamente el `Service` de Kubernetes
- Validación de salud activa (código HTTP + latencia) antes de conmutar tráfico
- **Rollback automático** (`if: failure()`) si la validación de salud falla

Se invoca desde el repo del proyecto así:

```yaml
jobs:
  usar-receta-compartida:
    uses: catalinapenamora/SDLC003D-SharedWorkflows-CatalinaPena/.github/workflows/deploy-api.yaml@main
    with:
      image-name: demo-api
      image-tag: ${{ github.ref_name }}
      k3s-server-public-ip: ${{ vars.K3S_SERVER_PUBLIC_IP }}
    secrets:
      DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
      DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
      EA2_SSH_PRIVATE_KEY: ${{ secrets.EA2_SSH_PRIVATE_KEY }}
```

## Documentación completa

Este repo contiene solo la plantilla. La explicación detallada de la arquitectura, la estrategia de despliegue Blue-Green y cómo se activa la remediación automática está documentada en el repo que consume este workflow:

 [SDLC003D-SharedClient-CatalinaPena...](https://github.com/catalinapenamora/SDLC003D-SharedClient-CatalinaPena)