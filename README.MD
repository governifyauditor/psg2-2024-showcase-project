
# Configurar un proyecto con tableros ZenHub en bluejay.

## 1. Crear y configurar el repositorio.

**1.1. Crear un repositorio en GitHub en la organizacion de la asignatura.**

**1.2. En la rama main añadir un archivo `info.yml`.**

info.yml:
```yaml
project:
  name: 'PSG2-2024-LX-XX'
  owner: 'LX'
  teamId: 'XX'
  identities: {}
  notifications:
    email: 'XXXXXXXXXXX'
  members:
    member1:
      name: 'Manuel'
      surname: 'Otero' 
      githubUsername: 'motero2k'
    member2:
      name: 'Javi'
      surname: 'Fernandez' 
      githubUsername: 'JaviFdez7'
    member3:
      name: 'XXXXXXXXXXX'
      surname: 'XXXXXXXXXXX' 
      githubUsername: 'XXXXXXXXXXX'
    member4:
      name: 'XXXXXXXXXXX'
      surname: 'XXXXXXXXXXX' 
      githubUsername: 'XXXXXXXXXXX'
```

- Rellenar las X.
    > Por ejemplo, el grupo 54 del Laboratorio 2:
    > name = PSG2-2024-L2-54
- Borrar los members que no se usen.

**1.3. Confirmar que la cuenta de governifyauditor es miembro de la organizacion de la asignatura.**

## 2. Configurar la asignatura en infrastructura.

**2.1. Acceder al assets [assets.bluejay.governify.io](assets.bluejay.governify.io).**

**2.2. Crear una nueva clase dentro del scope de development.**

- Accede al assets/private/scopes.json.
- Añade esto en `development[]`.
```json
        {
            "classId": "PSG2-2024",
            "identities": [],
            "hidden": false,
            "credentials": [],
            "projects": []
        },
```

**2.3. Configurar la plantilla de TPA.**

Esta plantilla se usará para crear todos los acuerdos de la asignatura.
- Navega hasta `public/renders/tpa`.
- Crea un archivo json con **el mismo nombre que el classId**: `PSG2-2024.json`.
- La plantilla se encuentra en [governify/zoo](https://github.com/governify/zoo/blob/main/bluejay/tpa/seville/PSG2-2223/v0.3/tpa-seville-PSG2-2223-v0.3-PROD.json).

**2.4 Configurar los cálculos automaticos.**

- Navega hasta `public/director`.
- Añade un archivo json y un js con **el mismo nombre que el classId**: `PSG2-2024.json` y `PSG2-2024.js`.
    PSG2-2024.json:
    ```json 
    {
        "init": "2024-01-01T00:00:00.000Z",
        "end": "2024-07-18T23:59:58.999Z",
        "interval": 3600000
    }
    ```

    PSG2-2024.js:
    ```js
    "use strict";
    const axios = require("axios");

    const today = new Date();
    const previousHour = new Date(today - 3600000);

    const period = [
        {
            from: previousHour.toISOString(),
            to: today.toISOString()
        }
    ];

    module.exports.main = (config) => {
        const requestURL = '$_[infrastructure.internal.reporter.default]/api/v4/contracts/' + config.agreementId + '/createPointsFromPeriods';
        console.log(period)
        axios.post(requestURL, {periods: period})
            .then(() => {
                console.log("Finished points creation for TPA:", config.agreementId);
            }).catch((error) => {
                console.log("Error when creating points for TPA:", config.agreementId, "\n", error);
            });
        return '';
    }


    ```

## 3. Unir el proyecto a la asignatura

- Accede a [join.bluejay.governify.io](join.bluejay.governify.io).
- Añade la **URL del repositorio** de GitHub.
- Click en **CHECK**. Si ha dado error revisa la [sintaxis](https://www.yamllint.com/) del info.yml.
- **Selecciona la clase** a la que te quieres unir (PSG2-2024).
- Click en **JOIN**.
- No pierdas el **enlace al dashboard**.

## 4.  Configurar ZH2GH.

Puedes segir la [guía de ZH2GH](https://docs.governify.io/development/services/zh2gh).

## 5. Crear el acuerdo y activar los calculos automaticos.

- Accede a [ui.bluejay.governify.io](ui.bluejay.governify.io).
- Selecciona la clase PSG2-2024.
- En el panel derecho, donde esté el proyecto click en **Create TPA**.
- Si la web no te ha redirigido a la vista del TPA, click en nuevo boton azul **TPA** al lado del nombre del proyecto.
- En la vista del TPA arriba a la derecha pulsar en **Automatic Computation**.
