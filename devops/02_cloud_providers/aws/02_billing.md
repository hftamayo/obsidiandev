
==esta experiencia es el relato, hallazgo y resolucion sobre cómo controlar el billing:

1. antes de probar el servicio EKS unicamente tenía un EC2 que por ratos la encendía, así mismo tenía un par de IAM Roles
2. despues que empezé a probar deployar 2 clusters de manera fallida y los dejé encendidos 3 días logré destruirlos, eliminé todos los recursos relacionados en VPC además de mi VM y aca un par de resultados

==saldo a las 16 horas del 04/09

![[Pasted image 20240904152928.png]]


==saldo a las 7:30 del 05/09

![[Pasted image 20240905075725.png]]

==que chingados es?

- punto bien importante es fijarse en que region vas a deployar
- en mi caso seleccioné la region us-west-2 para deployar los clusters
- ahora, revisando la VPC se supone que ya no tengo nada:

![[Pasted image 20240905083956.png]]

pero me fijé que dice US-East, al dar click en "See all regions"

![[Pasted image 20240905084150.png]]


y cabal!!!

![[Pasted image 20240905085600.png]]

![[Pasted image 20240905091025.png]]

![[Pasted image 20240905091229.png]]

![[Pasted image 20240905091324.png]]


![[Pasted image 20240905091435.png]]


![[Pasted image 20240905091622.png]]

este servicio pertenece a EC2, cuando se crea un cluster se asigna una IP Elastica:

![[Pasted image 20240905093655.png]]

![[Pasted image 20240905093836.png]]


==tabla de costos a las 20:32 del dia 05-09-2024

![[Pasted image 20240905203312.png]]

aca ya puedo ver que la diferencia son 0.50 y esta en el rubro de EC2-other

En Billing and Cost Management -> Bills abajo hay un detalle por cada serbicio:

![[Pasted image 20240905214913.png]]


![[Pasted image 20240905214933.png]]

==reporte a las 7:58 del 06-09:

![[Pasted image 20240906075858.png]]

## Setting up de un budget alert


Setting up a budget alert is the best option to monitor and control your AWS spending. AWS Budgets allows you to create custom cost and usage budgets, and you can set up alerts to notify you when your usage exceeds your specified thresholds.

### Steps to Set Up a Budget Alert

1. **Go to AWS Budgets**:
   - Open the [AWS Budgets Console](https://console.aws.amazon.com/billing/home#/budgets).

2. **Create a Budget**:
   - Click on "Create budget".
   - Choose "Cost budget" and click "Next".

3. **Configure Budget Details**:
   - Enter a name for your budget.
   - Set the period (e.g., monthly).
   - Specify the start date and, optionally, an end date.
   - Set the budgeted amount (e.g., $50).

4. **Set Up Alerts**:
   - Under "Configure alerts", set up notifications for when your actual or forecasted costs exceed a certain percentage of your budget.
   - For example, you can set an alert at 80% and 100% of your budget.
   - Enter the email addresses to receive the alerts.

5. **Review and Create**:
   - Review your budget settings and click "Create budget".

### Example: Setting Up a Budget Alert

1. **Navigate to AWS Budgets**:
   - Open the AWS Budgets Console.

2. **Create a Budget**:
   - Click "Create budget".
   - Select "Cost budget" and click "Next".

3. **Configure Budget**:
   - Name: `MyLambdaBudget`
   - Period: `Monthly`
   - Start Date: `Today`
   - Budgeted Amount: `$50`

4. **Set Up Alerts**:
   - Alert 1: 80% of budget
     - Threshold: `80%`
     - Notification Type: `Actual`
     - Email: `your-email@example.com`
   - Alert 2: 100% of budget
     - Threshold: `100%`
     - Notification Type: `Actual`
     - Email: `your-email@example.com`

5. **Review and Create**:
   - Review the settings and click "Create budget".

### Additional Tips

- **Enable Billing Alerts**: Ensure that billing alerts are enabled in your AWS account.
  - Go to the [Billing and Cost Management Dashboard](https://console.aws.amazon.com/billing/home#/).
  - Click on "Billing preferences" and enable "Receive Billing Alerts".

- **Monitor Usage**: Regularly monitor your AWS usage and costs through the AWS Management Console.

By setting up a budget alert, you can effectively monitor and control your AWS spending, ensuring that you stay within your budget and avoid unexpected charges.


![[Pasted image 20241001155339.png]]

![[Pasted image 20241001155437.png]]

![[Pasted image 20241001155548.png]]

![[Pasted image 20241001155646.png]]

![[Pasted image 20241001155708.png]]

![[Pasted image 20241001155907.png]]

![[Pasted image 20241001160007.png]]