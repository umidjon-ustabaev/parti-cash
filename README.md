AI challenge: PartiCash wep-app
During the development of this Rails app, we engaged with prominent LLM models including GPT-3 and GTP-4, notably leveraging chatbots like ChatGPT and GitHub Copilot. Below are the steps undertaken throughout the app's development, along with a brief conclusion highlighting the invaluable assistance provided by LLM models and how they streamlined the development process.

Developing the architecture

Since I have used the web framework Ruby on Rails, I did not need to think and decide on the architecture that much, by default framework uses Monolithic MVC architecture so in this step of development there was no any need to involve AI.
Setting up the Database and generating respective models

Setting up the schema: here I used GPT-3, I explained the business logic to the chat bot and asked to advice database schema, more the most part adviced schema contained all the required tables and columns, however they were really simple and missed the followings:
Respective table constraints
Uniq indexes for specific table columns like, external_ids, user_emails
B-tree indexes for loaded tables like products
Also some important tables for processing the payments and installments were missing
Configuration of the database

For the configuration part, there was no any of involvement of the chat bot since all the db configs are handled by the framework under the hood.
Creating respective models and defining required methods within

Models are provided by the framework (ORM) itself so there was no need to use the chat bot in this concern as well, however, for complex models like Charge associated with other models via polymorphic relation some tricky questions were asked just to see if the chat bot can direct in the right direction, but the chat bot confused more then helped
Setting up respective controllers and and creating endpoints

For the light controllers like just rendering one product, there was no need to use any chat bot since they are easly generated by the framework, however for Authentication via google as well as for webhook to receive confirmation from the PSP service I integrated copilot since much of a code should have been written.
Google Authentication: Copilot showed itself pretty good via autocompletion below code was purely generated by the bot, I only type the name of the controller:
    module Users  
      class OmniauthCallbacksController < Devise::OmniauthCallbacksController
         # POST /auth/google_oauth2
         def google_oauth2
           if (user = User.from_google_auth(api_params))
             return_to_path = session[:user_return_to]
             sign_out_all_scopes
             session[:user_return_to] = return_to_path
             sign_in_and_redirect user, event: :authentication
           else
             flash[:alert] = 'Failed to authenticate with Google'
             redirect_to user_session_path
           end
         end

         # GET|POST /auth/google_oauth2/callback

         # The path used when OmniAuth fails

         private

         def api_params
           @api_params ||= {
                             email: auth.info.email,
                             uid: auth.uid,
                             first_name: auth.info.first_name,
                             last_name: auth.info.last_name,
                             avatar_url: auth.info.image
                           }
         end

         def auth
           @auth ||= request.env['omniauth.auth']
         end
      end
   end
Generating additional service classes to handle more complex business logics

For this app one of the main business logic happens when the user places an order, many different models should be created like Order, Installment, InstallmentPayments. For this part of the app I needed to create a specific builder class which take care of all these steps within a one transaction
Copilot was a great help here as well, I explained the logic and received ready to use snippet, of course had to clear it up a bit but it was functional code and was written having in mind best practices overall.
Testing

Copilot is the greatest help when writing unit tests at least in Ruby, it literally saves all the time the dev can spent writing tests and allow the dev focus on writing the code to implement business logic below some of the benefits I got.
Allowed me to use TDD for development
Unit tests written by Copilot covers almost all scenarios and thus allows the dev to detect potential bugs before hand
Boosts the development
Better development experience
Conclusion
During the development of the PartiCash web application, the integration of prominent Language Model (LLM) technologies, specifically GPT-3 and GTP-4, along with chatbots like ChatGPT and GitHub Copilot, significantly influenced various stages of the development process. Here's a comprehensive overview of the impact and contributions of LLM models in each development phase:

Architecture Development: Given the utilization of Ruby on Rails framework, architectural decisions were largely streamlined, minimizing the need for AI involvement during this phase.

Database Schema Setup: While setting up the database schema, GPT-3 was employed to advise on the initial schema design. Although the suggested schema provided a foundation, it lacked certain essential components such as table constraints, unique indexes, and crucial tables related to payment processing.

Database Configuration: The configuration of the database was handled by the framework itself, thereby excluding the need for AI intervention in this aspect.

Model Creation and Definition: The framework's built-in ORM facilitated model creation, with minimal AI assistance required. However, for complex models such as those involving polymorphic relations, attempts to seek guidance from chatbots like ChatGPT proved to be more confusing than helpful.

Controller Setup and Endpoint Creation: While basic controllers were easily generated by the framework, complex tasks such as integrating Google authentication and webhook implementation for PSP service confirmation demanded more extensive code generation. GitHub Copilot proved to be instrumental in generating functional code snippets for these tasks, enhancing productivity and reducing development time.

Additional Service Class Generation: Developing service classes to handle intricate business logic, particularly during order processing, necessitated the creation of specialized builder classes. GitHub Copilot offered invaluable assistance by providing ready-to-use code snippets, albeit requiring some refinement to align with best practices.

Testing: GitHub Copilot emerged as an indispensable tool for writing comprehensive unit tests, enabling Test-Driven Development (TDD) practices. The AI-generated unit tests covered a wide range of scenarios, aiding in early bug detection and fostering a more efficient development process.

In conclusion, the integration of LLM models, particularly GitHub Copilot, significantly enhanced the development workflow of the PartiCash web application. While AI assistance proved invaluable in certain aspects such as code generation and testing, careful consideration and refinement were necessary to ensure alignment with project requirements and coding standards. Overall, leveraging LLM technologies contributed to a more streamlined and efficient development process, ultimately facilitating the successful implementation of the web application's functionalities.
