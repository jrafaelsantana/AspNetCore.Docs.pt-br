<span data-ttu-id="b862e-101">A [identidade](xref:security/authentication/identity) de ASP.NET Core é amplamente inafetada por [cookies SameSite](xref:security/samesite) , exceto por cenários avançados como o `IFrames` ou a `OpenIdConnect` integração.</span><span class="sxs-lookup"><span data-stu-id="b862e-101">ASP.NET Core [Identity](xref:security/authentication/identity) is largely unaffected by [SameSite cookies](xref:security/samesite) except for advanced scenarios like `IFrames` or `OpenIdConnect` integration.</span></span>

<span data-ttu-id="b862e-102">Ao usar o `Identity` , ***não*** adicione nenhum provedor ou chamada de cookie. o cuida disso ` services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)` `Identity` .</span><span class="sxs-lookup"><span data-stu-id="b862e-102">When using `Identity`, do ***not*** add any cookie providers or call ` services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)`, `Identity` takes care of that.</span></span>