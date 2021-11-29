# BPMN Tester

## Achtergrond

De business is eigenaar van alle business interfaces waaronder ook de bedrijfsprocessen. Als eigenaar moeten zij dan ook correcte processen in BPMN formaat opleveren. Processen bevatten ook logica, denk hierbij aan gateways en DMN modellen. Deze logica moet, net als alle andere logica, als unit automatisch getest kunnen worden.

Logica (expressies) worden in FEEL formaat geschreven. FEEL is een DMN standaard zoals [beschreven in de DMN specificatie.](https://www.omg.org/spec/DMN/). Zie ook [Camunda documentatie](https://docs.camunda.io/docs/reference/feel/what-is-feel/) voor meer informatie.

Camunda heeft een opensource [FEEL parser](https://camunda.github.io/feel-scala/) geschreven.

## Gewenst resultaat

Logica in een proces moet via een yaml test specificatie automatisch getest worden.

## Oplossingsrichting

* Een script zoek recursief naar alle *.bpmn.tests bestanden. Voor elke test bestand moet het bpmn bestand (- .tests extenstie) bestaan.
* Het .bpmn.tests bestand wordt gevalideerd op juistheid;
* Het .bpmn bestand wordt gevalideerd op juistheid (https://github.com/bpmn-io/bpmn-js-bpmnlint);
* Voor elke test word de logica opgezocht in de process definitie:
*   * de expressies van alle uitgaande gateway flows aan de hand van `when_executing_gateway`;
    * de DMN expressie op basis van `when_executing_decision`;
* Elke expressie en `given_state` wordt m.b.v. van de [FEEL implementatie](https://camunda.github.io/feel-scala/) uitgevoerd.
* Het uitkomst wordt vergeleken met het verwachte resultaat: `expect_flow_activated` of `expect_result`

Het formaat beschrijft naam van de test, de proces data, het id van de gateway of decision model dat uitgevoerd moet worden en het verwachte resultaat (geactiveerde route of resultaat).

```yaml
- scenario: name_of_test_scenario 
  given_state: state_object    
  when_executing_gateway: id_of_gateway
  expect_flow_activated: id_of_flow
  
- scenario: name_of_test_scenario 
  given_state: state_object    
  when_executing_decision_model: id_of_decision_model
  expect_result: result_object

```

Ze voor een voorbeeld implementatie de volgende bestanden:

* https://github.com/synionnl/website/blob/feature/bot/docs/blog/bizdevops/living-documentation/process.feature
* https://github.com/arjangeertsema/hexagonal-architecture/blob/main/Domain/Domain.UseCases.Tests/Steps/ProcessStepDefintions.cs

## Delevirables

* Test script
* Docker file
* Github workflow action
* Test resultaat als te dowloaden json bestand




