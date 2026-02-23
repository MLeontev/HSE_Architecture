FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["InterviewSimulator.API/InterviewSimulator.API.csproj", "InterviewSimulator.API/"]
COPY ["CodeExecution.Infrastructure.Implementation.DataAccess/CodeExecution.Infrastructure.Implementation.DataAccess.csproj", "CodeExecution.Infrastructure.Implementation.DataAccess/"]
COPY ["CodeExecution.Domain/CodeExecution.Domain.csproj", "CodeExecution.Domain/"]
COPY ["CodeExecution.Infrastructure.Interfaces.DataAccess/CodeExecution.Infrastructure.Interfaces.DataAccess.csproj", "CodeExecution.Infrastructure.Interfaces.DataAccess/"]
COPY ["CodeExecution.Infrastructure.Workers/CodeExecution.Infrastructure.Workers.csproj", "CodeExecution.Infrastructure.Workers/"]
COPY ["CodeExecution.IntegrationEvents/CodeExecution.IntegrationEvents.csproj", "CodeExecution.IntegrationEvents/"]
COPY ["CodeExecution.UseCases/CodeExecution.UseCases.csproj", "CodeExecution.UseCases/"]
COPY ["CodeExecution.Infrastructure.Interfaces.CodeExecution/CodeExecution.Infrastructure.Interfaces.CodeExecution.csproj", "CodeExecution.Infrastructure.Interfaces.CodeExecution/"]
COPY ["Framework.Domain/Framework.Domain.csproj", "Framework.Domain/"]
COPY ["CodeExecution.Presentation/CodeExecution.Presentation.csproj", "CodeExecution.Presentation/"]
COPY ["CodeExecution.Infrastructure.Implementation.CodeExecution.Docker/CodeExecution.Infrastructure.Implementation.CodeExecution.Docker.csproj", "CodeExecution.Infrastructure.Implementation.CodeExecution.Docker/"]
COPY ["Interview.Infrastructure.Implementation.DataAccess/Interview.Infrastructure.Implementation.DataAccess.csproj", "Interview.Infrastructure.Implementation.DataAccess/"]
COPY ["Interview.Infrastructure.Interfaces.DataAccess/Interview.Infrastructure.Interfaces.DataAccess.csproj", "Interview.Infrastructure.Interfaces.DataAccess/"]
COPY ["Interview.Domain/Interview.Domain.csproj", "Interview.Domain/"]
COPY ["QuestionBank.Infrastructure.Implementation.DataAccess/QuestionBank.Infrastructure.Implementation.DataAccess.csproj", "QuestionBank.Infrastructure.Implementation.DataAccess/"]
COPY ["QuestionBank.Domain/QuestionBank.Domain.csproj", "QuestionBank.Domain/"]
COPY ["QuestionBank.Infrastructure.Interfaces.DataAccess/QuestionBank.Infrastructure.Interfaces.DataAccess.csproj", "QuestionBank.Infrastructure.Interfaces.DataAccess/"]
COPY ["Users.Infrastructure.Implementation.DataAccess/Users.Infrastructure.Implementation.DataAccess.csproj", "Users.Infrastructure.Implementation.DataAccess/"]
COPY ["Users.Infrastructure.Interfaces.DataAccess/Users.Infrastructure.Interfaces.DataAccess.csproj", "Users.Infrastructure.Interfaces.DataAccess/"]
COPY ["Users.Domain/Users.Domain.csproj", "Users.Domain/"]
COPY ["QuestionBank.Presentation/QuestionBank.Presentation.csproj", "QuestionBank.Presentation/"]
COPY ["Framework.Presentation/Framework.Presentation.csproj", "Framework.Presentation/"]
COPY ["QuestionBank.UseCases/QuestionBank.UseCases.csproj", "QuestionBank.UseCases/"]
COPY ["Interview.UseCases/Interview.UseCases.csproj", "Interview.UseCases/"]
COPY ["QuestionBank.ModuleContract/QuestionBank.ModuleContract.csproj", "QuestionBank.ModuleContract/"]
COPY ["QuestionBank.ModuleContract.Implementation/QuestionBank.ModuleContract.Implementation.csproj", "QuestionBank.ModuleContract.Implementation/"]
COPY ["Interview.Presentation/Interview.Presentation.csproj", "Interview.Presentation/"]
RUN dotnet restore "InterviewSimulator.API/InterviewSimulator.API.csproj"
COPY . .
WORKDIR "/src/InterviewSimulator.API"
RUN dotnet build "./InterviewSimulator.API.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./InterviewSimulator.API.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "InterviewSimulator.API.dll"]
