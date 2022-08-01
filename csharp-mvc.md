# ASP Core Identity

## With MongoDB
The below tutorial shows how to use a MongoDB NoSQL DB for Core Identity, instead of normal SQL.

Ref: https://www.yogihosting.com/aspnet-core-identity-mongodb/


## Roles
The below tutorial shows how to create/edit/modify roles using ASP.NET Core Identity.

Ref: https://www.yogihosting.com/aspnet-core-identity-roles/#add-remove-users


## Email Confirmation
This tutorial show how to confirm emails via Core Identity. However, SendGrid is not used as the actual email sender.

Ref: https://docs.microsoft.com/en-us/aspnet/mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset#email-confirmation



### Two Factor Token Provider
Provider class - obviously needs tweaking to generate more unique token
``` csharp
public class HedgehogEmailTwoFactorAuthentication<TUser> : IUserTwoFactorTokenProvider<TUser> where TUser : HedgehogUserAccount
    {
        public Task<bool> CanGenerateTwoFactorTokenAsync(UserManager<TUser> manager, TUser user)
        {
            if (manager != null && user != null)
            {
                return Task.FromResult(true);
            }
            else
            {
                return Task.FromResult(false);
            }
        }

        // Genereates a simple token based on the user id, email and another string.
        private string GenerateToken(HedgehogUserAccount user, string purpose)
        {
            string secretString = "coffeIsGood";
            return secretString + user.Email + purpose + user.Id;
        }

        public Task<string> GenerateAsync(string purpose, UserManager<TUser> manager, TUser user)
        {
            return Task.FromResult(GenerateToken(user, purpose));
        }

        public Task<bool> ValidateAsync(string purpose, string token, UserManager<TUser> manager, TUser user)
        {
            return Task.FromResult(token == GenerateToken(user, purpose));
        }
    }
```

Add to service in MVC
``` csharp
services.AddIdentityCore<CustomerAccount>(options => options.SignIn.RequireConfirmedAccount = true)
    .AddRoles<IdentityRole>()
    .AddEntityFrameworkStores<ApplicationDbContext>()
    .AddTokenProvider("Default", typeof(HedgehogEmailTwoFactorAuthentication<CustomerAccount>))
    .AddDefaultUI();
```

Ref: https://stackoverflow.com/questions/67361087/no-iusertwofactortokenprovidertuser-named-default-is-registered
