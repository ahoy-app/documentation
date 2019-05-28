---
title: Ahoy App
author: Sergio Martín Segura
documentclass: scrartcl
toc: true
toc-title: true
toc-depth: 2
---

# Introducción y Objetivos

El objetivo de este proyecto es diseñar e implementar un sistema distribuido, concretamente, una plataforma de chat en tiempo real.

Los principios fundamentales de este sistema serán:

- Aproximación _Cloud Native_: La distribución en la nube será un aspecto a considerar desde el inicio del diseño.
- Escalabilidad horizontal: Se tratará de maximizar el paralelismo.
- Alta disponibilidad: Se tratarán de minimizar los puntos de fallo único buscando replicación en los servicios críticos.
- Minimizar deuda técnica: Como un proyecto que aspira a un ciclo de vida largo, se buscará minimizar las decisiones corto-placistas que repercutan de forma negativa en la progresión y mantenimiento del proyecto.
